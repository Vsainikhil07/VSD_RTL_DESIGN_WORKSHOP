# Physical Design – Module 2

## Chip Floorplanning, Power Integrity and Physical Implementation

---

## 📌 Module Overview

This module focuses on the early physical-design stages of an ASIC, beginning with the interpretation of a logical netlist and progressing toward floorplanning, power planning, cell placement, and physical layout.

The practical work explores how logical cells are represented using technology-library cells, how their area influences the core dimensions, and how utilization and aspect ratio are used to construct a suitable floorplan.

The module also introduces power-integrity concepts such as switching current, IR drop, inductive voltage variation, noise margin, and decoupling capacitors. These concepts are connected to the physical implementation of the power-distribution network.

The practical portion uses the SKY130 technology and OpenLane/OpenROAD environment to examine floorplan configuration, LEF files, power structures, standard-cell placement, placement blockages, tap cells, timing constraints, and logical-to-physical cell mapping.

The overall implementation can be viewed as:

**Netlist → Cell Characterization → Area Estimation → Floorplan → Power Network → Placement → Physical Layout**

---

## 🎯 Learning Objectives

By completing this module, the following concepts are explored:

* Analyze the contents of a digital netlist before physical implementation.
* Understand the physical representation of logical standard cells.
* Estimate the area occupied by the design.
* Relate cell area to core dimensions and utilization.
* Understand the effect of aspect ratio on floorplan geometry.
* Identify the role of fixed cells and IP blocks.
* Study the effect of block placement on routing and timing.
* Understand sources of voltage drop in a power network.
* Analyze the effect of switching current on supply stability.
* Understand digital noise margins.
* Study the purpose of decoupling capacitors.
* Understand power-grid and PDN organization.
* Configure floorplanning parameters in OpenLane.
* Examine SKY LEF and technology information.
* Understand standard-cell placement and density.
* Study placement blockages and tap-cell insertion.
* Apply timing constraints using SDC.
* Observe the mapping between logical cells and physical library cells.
* Inspect the implemented design using OpenROAD/layout tools.

---

## 🛠️ Design Environment

| Component               | Role in the Module                                   |
| ----------------------- | ---------------------------------------------------- |
| OpenLane                | Automated ASIC implementation flow                   |
| OpenROAD                | Physical-design implementation and layout inspection |
| SKY130 PDK              | Technology and process information                   |
| sky130_fd_sc_hd         | Standard-cell library                                |
| Verilog                 | Hardware design and netlist description              |
| Tcl                     | Flow and implementation configuration                |
| LEF                     | Abstract physical cell/library information           |
| SDC                     | Timing constraint description                        |
| KLayout / Layout Viewer | Physical-layout inspection                           |
| Linux / Ubuntu          | Execution environment                                |
| GitHub                  | Documentation and project management                 |

---

# 1. Understanding the Starting Netlist

Physical design begins with a logical description of the circuit.

The netlist represents the circuit using interconnected logic elements such as flip-flops, combinational gates, clock connections, and data paths.

Before starting floorplanning, the contents of the netlist must be understood because the number and type of cells influence the physical area required by the design.

**Netlist representation and logical components**
<img width="1072" height="603" alt="image" src="https://github.com/user-attachments/assets/4712725c-05d1-46f4-a298-6653c3f6d395" />


---

# 2. From Logic Symbols to Physical Cells

Logical cells do not directly represent their physical dimensions.

During physical implementation, each logical element is associated with a physical standard cell from the technology library. The physical cell contains information about its dimensions, pins, power connections, and placement requirements.

For example, logical flip-flops and logic gates are represented using corresponding cells from the SKY130 standard-cell library.

This creates the connection between the logical design and its physical implementation.



---

# 3. Determining the Required Cell Area

Once the physical representation of the cells is understood, the total area occupied by the cells can be estimated.

For a simplified unit-cell example:

$$
Area = Width \times Height
$$

If:

* Width = 1 unit
* Height = 1 unit

then:

$$
Area = 1 \times 1 = 1\ square\ unit
$$

The area of individual cells can then be accumulated to obtain the approximate total cell area of the design.

**Cell-area calculation**
<img width="1082" height="608" alt="image" src="https://github.com/user-attachments/assets/6d8bc767-e955-4f43-8f78-dab02c9eca18" />


---

# 4. Establishing Core Utilization

The total cell area alone is not sufficient to determine the core size.

Additional space is required for routing, buffers, power structures, and physical optimization.

The utilization factor is therefore defined as:

$$
Utilization = \frac{Occupied\ Cell\ Area}{Available\ Core\ Area}
$$

A very high utilization leaves little whitespace and can result in routing congestion. A lower utilization provides additional physical flexibility but increases the required chip area.

**Utilization-factor representation**
<img width="1076" height="592" alt="image" src="https://github.com/user-attachments/assets/745063be-28cb-4d39-b255-e1857b6af08f" />


---

# 5. Controlling Floorplan Shape

The shape of the core is controlled using the aspect ratio:

$$
Aspect\ Ratio = \frac{Height}{Width}
$$

Changing the aspect ratio changes the geometry of the floorplan even when the overall area remains similar.

The selected dimensions must provide adequate space for cell placement, routing, power structures, and other physical requirements.

** Aspect-ratio and floorplan geometry example**
<img width="1076" height="592" alt="image" src="https://github.com/user-attachments/assets/745063be-28cb-4d39-b255-e1857b6af08f" />

---

# 6. Core and Die Relationship

The **core** is the main implementation region, while the **die** represents the complete chip boundary surrounding the core.

The selected core and die dimensions depend on:

* Total cell area
* Target utilization
* Aspect ratio
* I/O requirements
* Routing requirements
* Power-distribution structures

Providing sufficient whitespace between physical structures is important for later implementation stages.

**Core and die dimension example**
<img width="982" height="398" alt="image" src="https://github.com/user-attachments/assets/9d8c716d-d331-4e36-b266-20d52bdbc93e" />


---

# 7. Planning Fixed Physical Structures

Not every structure in an ASIC can be freely moved during automated placement.

Large or special-purpose blocks are assigned fixed locations during floorplanning.

Examples include:

* Memory blocks
* Large IPs
* Special-purpose logic
* Clock-related structures
* Other macros

Their positions must be decided before regular standard-cell placement.

** Pre-placed/fixed structures**
<img width="1050" height="347" alt="image" src="https://github.com/user-attachments/assets/faa46bf9-edd8-4f54-861a-e94b4ed615ac" />

---

# 8. Connectivity-Driven Block Arrangement

The location of fixed blocks affects the rest of the design.

Blocks with strong connectivity may need to be positioned closer together to avoid unnecessary routing distance.

Poor block organization can lead to:

* Longer wires
* Routing congestion
* Timing problems
* Routing detours

Therefore, floorplanning is not simply about fitting blocks inside the die; it also considers how the blocks communicate with each other.

** Placement and connectivity of pre-placed cells**
<img width="692" height="586" alt="image" src="https://github.com/user-attachments/assets/90e45278-1356-47da-91ed-dd09536264c7" />


---

# 9. Organizing IPs Within the Floorplan

Modern ASICs frequently contain pre-designed IP blocks.

These blocks occupy larger areas than ordinary standard cells and therefore need to be considered early in the physical design.

The floorplan determines:

* IP location
* Available routing channels
* Standard-cell regions
* Power-access regions
* Connection paths between blocks

A well-organized IP arrangement simplifies subsequent placement and routing.

**IP blocks arranged inside the floorplan**
<img width="1017" height="533" alt="image" src="https://github.com/user-attachments/assets/f16f861c-9343-4f72-862a-32e081794300" />


---

# 10. Local Supply Support Using Decaps

High-switching blocks may require large instantaneous current.

Decoupling capacitors, or **decap cells**, can be positioned near such regions to provide temporary local charge during switching events.

Their placement around important blocks reduces the distance between the temporary charge source and the circuit requiring current.

**Decoupling capacitors around pre-placed blocks**
<img width="997" height="738" alt="image" src="https://github.com/user-attachments/assets/d036ad73-42f0-4954-a9ea-4af6f4f968cd" />

---

# 11. Understanding Current-Induced Voltage Drop

When a large number of cells switch simultaneously, the power network experiences increased current demand.

The resistance of the power network produces a voltage drop according to:

$$
V = IR
$$

This is commonly associated with **IR drop**.

A significant voltage drop can reduce the supply voltage available to the circuit and may affect reliable operation.

** Switching current and resistive voltage drop**
<img width="1081" height="576" alt="image" src="https://github.com/user-attachments/assets/b6ad9e7d-42fd-4d14-85bd-e039a40e7a77" />

---

# 12. Effect of Inductance During Switching

The power network also contains parasitic inductance.

When current changes rapidly, the resulting inductive voltage variation can be represented as:

$$
V = L\frac{di}{dt}
$$

Therefore, circuits with rapid changes in current can experience additional supply-voltage disturbances.

Both resistance and inductance must therefore be considered when analyzing power integrity.

**Inductive voltage variation**
<img width="1081" height="576" alt="image" src="https://github.com/user-attachments/assets/b6ad9e7d-42fd-4d14-85bd-e039a40e7a77" />
---

# 13. Evaluating Digital Noise Tolerance

Digital circuits must tolerate a certain amount of unwanted voltage disturbance without incorrectly changing their logic state.

This capability is represented using noise margins.

### High-Level Noise Margin

$$
NM_H = V_{OH(min)} - V_{IH(min)}
$$

### Low-Level Noise Margin

$$
NM_L = V_{IL(max)} - V_{OL(max)}
$$

A disturbance smaller than the available noise margin can generally be tolerated without causing a logic error.

** Noise margin and noise-bump representation**
<img width="1062" height="570" alt="image" src="https://github.com/user-attachments/assets/b2dd3b9b-bc72-4501-86ba-0849729f33b1" />

---

# 14. Using Decoupling to Improve Supply Stability

A decoupling capacitor provides a local charge source during short-duration current-demand events.

The basic sequence is:

**Switching event**
↓
**Current demand increases**
↓
**Decap supplies local charge**
↓
**Power network restores the stored charge**

This reduces the severity of transient supply fluctuations.

**Decoupling capacitor as a power-integrity solution**
<img width="1082" height="577" alt="image" src="https://github.com/user-attachments/assets/1599fc12-997d-4ff8-b6ab-1f0784eb6703" />

---

# 15. Strategic Distribution of Decap Cells

Decaps can be distributed around high-activity blocks rather than being concentrated at a single location.

A suitable distribution considers:

* Location of switching blocks
* Available whitespace
* Power-grid structure
* Distance from the load
* Local current requirements

Placing decaps closer to the affected circuit helps provide more effective local supply support.

**Decap placement around multiple blocks**
<img width="1065" height="457" alt="image" src="https://github.com/user-attachments/assets/0dac6a41-351e-458d-a642-1335ba98b4cb" />

**Decap arrangement within the floorplan**
<img width="1017" height="610" alt="image" src="https://github.com/user-attachments/assets/1654ee6b-37ab-4290-83d7-6c5a6a95fc62" />

---

# 16. Power Network and Simultaneous Switching

A physical design contains multiple drivers, loads, and signal paths connected to a common power network.

When several signals switch together, the resulting current demand can increase significantly.

A multi-bit bus is a useful example because several signal lines may transition during the same operation.

Therefore, the power network must be designed to handle both normal and transient current requirements.

** Driver, load, power network and 16-bit bus**
<img width="1037" height="566" alt="image" src="https://github.com/user-attachments/assets/9bd89ef6-d8a4-47aa-9f4f-6a3100113351" />

---

# 17. Building the Initial Floorplan

After understanding the physical requirements, the floorplan is created.

Important parameters include:

* Core utilization
* Aspect ratio
* Die area
* I/O placement
* Core boundaries
* I/O metal layers

OpenLane exposes these settings through configuration variables such as:

`FP_CORE_UTIL`

`FP_ASPECT_RATIO`

`FP_SIZING`

`DIE_AREA`

`FP_IO_HMETAL`

`FP_IO_VMETAL`



---

# 18. Constructing the Power Grid

Power planning creates the physical structures used to distribute supply and ground throughout the chip.

The power network typically contains:

* VDD rails
* VSS rails
* Horizontal straps
* Vertical straps
* Standard-cell power connections

The grid provides multiple paths for delivering power to cells throughout the core.

**Power planning and power-grid structure**
<img width="1027" height="622" alt="image" src="https://github.com/user-attachments/assets/087dd97c-ed5c-4ef4-9807-91d4bab84c0f" />

---

# 19. Understanding the PDN Structure

The Power Distribution Network connects the chip-level power supply to the standard-cell power rails.

A typical PDN contains intersecting horizontal and vertical metal structures.

A strong PDN helps:

* Reduce voltage drop
* Improve supply uniformity
* Support transient current demand
* Maintain power integrity

** PDN structure**
<img width="1021" height="610" alt="image" src="https://github.com/user-attachments/assets/a15c9a9c-29df-4200-b334-8d7f6b0cdad9" />

---

# 20. Running the PicoRV32A Physical-Design Flow

The practical implementation uses the PicoRV32A design with OpenLane.

The automated flow connects several physical-design stages:

**RTL → Synthesis → Floorplanning → Placement → Clock Tree → Routing → GDSII**

The OpenLane configuration controls important parameters used throughout this process.

** PicoRV32A OpenLane flow**
<img width="510" height="501" alt="image" src="https://github.com/user-attachments/assets/8e425fb2-c679-43d2-8e14-fabed92bb7f4" />

---

# 21. Controlling Implementation Through Tcl

OpenLane uses Tcl configuration variables to define design and physical-design requirements.

Important parameters include:

* Design name
* Verilog source
* SDC file
* Clock period
* Clock port
* Core utilization
* Placement density

For example:

```tcl
set ::env(DESIGN_NAME) "picorv32a"
set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"
set ::env(FP_CORE_UTIL) 50
```

These settings provide the implementation tools with the constraints needed to construct the physical design.

**OpenLane configuration**
<img width="461" height="458" alt="image" src="https://github.com/user-attachments/assets/711d9bf5-833a-4c82-8090-781e8fdd2587" />

---

# 22. Reading Physical Information from SKY130

The SKY130 PDK provides the technology information required for physical implementation.

The standard-cell library used in this work is:

`sky130_fd_sc_hd`

The library contains physical cells that can be selected during implementation.

LEF files provide an abstract representation of these cells without requiring the complete detailed layout.

**SKYWAFER standard-cell library configuration**
<img width="767" height="757" alt="image" src="https://github.com/user-attachments/assets/25b3a34c-b583-42eb-98af-3e5db70385ef" />

---

# 23. Examining LEF and Technology Data

LEF files contain physical information required by placement and routing tools.

This includes:

* Cell dimensions
* Pin locations
* Metal layers
* Obstructions
* Placement sites

Technology files provide information about physical layers and manufacturing-related requirements.

The manufacturing grid and site definitions ensure that cells and physical geometries are positioned according to technology rules.

** SKYWAFER LEF information**
<img width="725" height="731" alt="image" src="https://github.com/user-attachments/assets/c797f147-a033-4a86-a7b1-16d3c3b702b4" />

---

# 24. Configuring the OpenLane Floorplan

OpenLane provides several parameters for controlling the floorplan.

### Core and Die Parameters

* `FP_CORE_UTIL`
* `FP_ASPECT_RATIO`
* `FP_SIZING`
* `DIE_AREA`

### I/O Parameters

* `FP_IO_HMETAL`
* `FP_IO_VMETAL`
* `FP_IO_MODE`

### PDN Parameters

* `FP_PDN_VOFFSET`
* `FP_PDN_HOFFSET`
* `FP_PDN_VPITCH`
* `FP_PDN_HPITCH`

### Physical Support Cells

* `FP_WELLTAP_CELL`
* `FP_ENDCAP_CELL`
* `FP_TAPCELL_DIST`

These parameters allow the generated floorplan to be adapted to the requirements of the design.



---

# 25. Placing Standard Cells

Once the floorplan and power structures are established, regular standard cells are placed inside the available core region.

The placement process considers:

* Cell connectivity
* Timing requirements
* Placement rows
* Cell density
* Blockages
* Available routing resources

Good placement reduces unnecessary wire length and helps improve timing and routability.

** Standard-cell placement**
<img width="1086" height="533" alt="image" src="https://github.com/user-attachments/assets/325332bb-4c79-40cb-bb4c-7193ee9cab9d" />

**Placement command/output**
<img width="772" height="747" alt="image" src="https://github.com/user-attachments/assets/a415d6a9-9270-4475-9c09-6f825d9b5764" />
<img width="815" height="402" alt="image" src="https://github.com/user-attachments/assets/6cc7741f-3041-4221-aa05-0157d5649a58" />

---

# 26. Reserving Regions with Placement Blockages

Some areas of the floorplan must remain unavailable to ordinary standard cells.

Placement blockages can reserve these regions for:

* Macros
* Decap cells
* Special cells
* Routing resources
* Future physical structures

This prevents the placement engine from occupying areas that have been intentionally reserved.


---

# 27. Adding Decaps and Tap Cells

Two important special-cell structures considered during physical implementation are decaps and tap cells.

### Decap Cells

Decaps support local power stability during transient switching events.

### Tap Cells

Tap cells provide the required well/substrate connections and help reduce the possibility of latch-up.

Their insertion is controlled according to the requirements of the selected technology library.


---

# 28. Applying Timing Constraints

Physical implementation must consider the timing requirements of the design.

The SDC file can define the clock and other timing constraints.

For example:

```tcl
create_clock \
    -name clk \
    -period 5.0 \
    [get_ports clk]
```

The clock period provides the target timing requirement used during timing-aware implementation.


---

# 29. Mapping Logical Cells to Physical Library Cells

A logical netlist contains abstract cell instances, while physical implementation requires actual cells from the technology library.

The mapping process connects the two representations:

**Logical Cell → Technology Library Cell → Physical Cell Instance**

For example, a logical flip-flop is associated with a suitable flip-flop cell from the SKY130 library.

This mapping provides the physical dimensions and pin information required for placement.

** Logical-to-physical cell mapping**
<img width="1087" height="557" alt="image" src="https://github.com/user-attachments/assets/40c75d11-d2b7-4733-ab8c-9c55f4aafd4d" />





---

# 30. Final Physical Placement

After the cells have been mapped to physical library cells, they are positioned within the floorplan.

The final placement is influenced by:

* Connectivity
* Timing
* Congestion
* Placement rows
* Blockages
* Fixed cells
* I/O locations

The resulting layout represents the logical circuit as actual physical cell instances.

** Final physical placement**
<img width="1068" height="596" alt="image" src="https://github.com/user-attachments/assets/4b292fa9-3cff-4bef-84d5-369cbd4b6ef8" />

---

## 🔑 Key Learnings

This module provided practical exposure to the relationship between logical design and physical implementation.

The major concepts covered include:

* Netlist interpretation
* Physical standard-cell representation
* Cell-area estimation
* Core and die sizing
* Utilization and aspect ratio
* Pre-placed blocks
* IP floorplanning
* Switching-current effects
* IR drop
* Inductive voltage variation
* Noise margin
* Decoupling capacitors
* Power planning
* PDN organization
* OpenLane configuration
* SKYWAFER libraries
* LEF and technology files
* Standard-cell placement
* Placement blockages
* Tap cells
* Timing constraints
* Logical-to-physical mapping
* OpenROAD layout inspection

---

## 📌 Practical Flow Summary

The complete practical progression studied in this module can be summarized as:

**Netlist Analysis**
↓
**Physical Cell Representation**
↓
**Area Estimation**
↓
**Core & Die Definition**
↓
**Floorplan Organization**
↓
**Power Planning**
↓
**PDN Creation**
↓
**Standard-Cell Placement**
↓
**Special-Cell / Tap-Cell Integration**
↓
**Timing Constraints**
↓
**Logical-to-Physical Mapping**
↓
**OpenROAD Physical Layout**

---

## ✅ Conclusion

This module demonstrates how the logical description of an ASIC is converted into an organized physical structure.

The study begins with cell identification and area estimation and progresses toward core and die definition, block organization, power planning, and standard-cell placement. Power-integrity concepts explain why the physical power network must be carefully designed to handle switching current and voltage variations.

The OpenLane and SKY130 environment provides practical exposure to floorplan configuration, technology libraries, LEF information, placement constraints, special cells, and timing requirements. OpenROAD then provides a physical view of the implemented design.

Overall, the module establishes the connection between **logical circuit structure, physical dimensions, power integrity, placement, and the final physical representation of the ASIC**.
