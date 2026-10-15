
# VSDBabySoC Physical Design and Multi-Corner Timing Analysis Using OpenROAD

**Floorplanning, Placement, Clock Tree Synthesis, Routing, and 16-Corner Post-Route STA**

**Author:** Vadla Sai Nikhil  
**Institution:** Anurag University  
**Branch:** Electronics and Communication Engineering (ECE), 3rd Year

---

## 1. Project Overview

This project presents the physical design analysis and post-route Static Timing Analysis (STA) of VSDBabySoC using OpenROAD-flow-scripts and the SkyWater SKY130HD technology.

The design was inspected through floorplanning, placement, Clock Tree Synthesis (CTS), and routing. The OpenROAD GUI was used to examine the floorplan, DAC macro, power distribution network (PDN), standard-cell placement, VDD power net, clock tree, and routed interconnect.

Post-route timing reports were analysed across 16 Process, Voltage, and Temperature (PVT) corners. The worst setup and hold slack values were extracted and compared to identify timing-critical operating conditions.

The analysis shows that hold timing is met at all 16 reported corners, while setup timing violations occur at five corners.

### Project Information

| Parameter | Details |
|---|---|
| Design | VSDBabySoC |
| Physical design tool | OpenROAD |
| Flow | OpenROAD-flow-scripts |
| Technology | SkyWater SKY130HD |
| Timing engine | OpenSTA |
| Environment | GitHub Codespaces with noVNC |
| Physical design stages | Floorplan, placement, CTS, routing |
| Timing corners | 16 |
| Analysis type | Post-route STA |
| Worst setup slack | -29.05 ns |
| Minimum hold slack | 0.19 ns |
| Setup violations | 5 corners |
| Hold violations | 0 corners |

## 2. Introduction

Physical design converts a synthesised digital circuit into a physical layout containing standard cells, macros, power distribution structures, and routed interconnect.

Static Timing Analysis evaluates the timing behaviour of a design without requiring functional simulation vectors. It checks whether data reaches its destination within the required timing limits.

In this project, VSDBabySoC was analysed using OpenROAD-flow-scripts with the SKY130HD platform. The floorplan, placement, clock-tree construction, and routed layout were inspected, followed by post-route timing analysis across 16 PVT corners.

The results demonstrate how operating conditions affect setup and hold timing and help identify corners requiring further optimisation.

## 3. Physical Design Files and Directories

The analysis used the OpenROAD-flow-scripts environment, generated design databases, timing constraints, and technology configuration files.

| Item | Path |
|---|---|
| SKY130HD platform | `./platforms/sky130hd/` |
| RC configuration | `./platforms/sky130hd/setRC.tcl` |
| Floorplan database | `./results/sky130hd/vsdbabysoc/base/2_floorplan.odb` |
| Floorplan constraints | `./results/sky130hd/vsdbabysoc/base/2_floorplan.sdc` |
| Routed timing constraints | `./results/sky130hd/vsdbabysoc/base/5_route.sdc` |
| Post-route STA reports | `~/Desktop/postroute_sta/` |
| Standard-cell platform | `sky130_fd_sc_hd` |

*The paths shown are based on the supplied commands and should be checked against the local workspace.*

### 3.1 Floorplan Database

The floorplan database contains the physical design information available at the floorplanning stage.

File: `2_floorplan.odb`

It was loaded to inspect the core area, macro placement, and power distribution network.

### 3.2 Timing Constraints

The SDC files specify timing requirements for the relevant physical design stages.

Files:

- `2_floorplan.sdc`
- `5_route.sdc`

### 3.3 SKY130HD RC Configuration

File: `setRC.tcl`

This configuration provides routing-layer resistance and capacitance parameters for parasitic estimation and timing analysis.

### 3.4 Post-Route Timing Reports

Directory: `~/Desktop/postroute_sta/`

The directory contains corner-specific reports used to extract worst setup and hold slack values.

## 4. Understanding the 16 Timing Corners

A PVT corner represents a combination of semiconductor process behaviour, supply voltage, and temperature.

- **FF (Fast-Fast):** Represents fast process behaviour and is commonly important for hold analysis.
- **SS (Slow-Slow):** Represents slow process behaviour and is commonly important for setup analysis.
- **TT (Typical-Typical):** Represents typical process behaviour.

Voltage and temperature also affect cell delay. Multi-corner analysis helps identify operating conditions where timing requirements may not be satisfied.

### 4.1 Complete Timing-Corner List

| No. | Timing Corner | Process | Temperature | Voltage |
|---:|---|---|---:|---:|
| 1 | `ff_100C_1v65` | FF | 100°C | 1.65 V |
| 2 | `ff_100C_1v95` | FF | 100°C | 1.95 V |
| 3 | `ff_n40C_1v56` | FF | -40°C | 1.56 V |
| 4 | `ff_n40C_1v65` | FF | -40°C | 1.65 V |
| 5 | `ff_n40C_1v76` | FF | -40°C | 1.76 V |
| 6 | `ff_n40C_1v95` | FF | -40°C | 1.95 V |
| 7 | `ss_100C_1v40` | SS | 100°C | 1.40 V |
| 8 | `ss_100C_1v60` | SS | 100°C | 1.60 V |
| 9 | `ss_n40C_1v28` | SS | -40°C | 1.28 V |
| 10 | `ss_n40C_1v35` | SS | -40°C | 1.35 V |
| 11 | `ss_n40C_1v40` | SS | -40°C | 1.40 V |
| 12 | `ss_n40C_1v44` | SS | -40°C | 1.44 V |
| 13 | `ss_n40C_1v60` | SS | -40°C | 1.60 V |
| 14 | `ss_n40C_1v76` | SS | -40°C | 1.76 V |
| 15 | `tt_025C_1v80` | TT | 25°C | 1.80 V |
| 16 | `tt_100C_1v80` | TT | 100°C | 1.80 V |

### 4.2 Importance of Multi-Corner Analysis

A design may meet timing under typical operating conditions but fail at other corners. Checking multiple PVT corners helps evaluate the design over its intended operating range.

The reported FF and TT corners meet setup timing. Five SS corners exhibit negative setup slack, indicating that further investigation is needed.

## 5. Physical Design Flow

### 5.1 Floorplanning

Floorplanning defines the core area, macro locations, placement rows, routing tracks, and power distribution network.

The floorplan database and timing constraints were loaded using:

```tcl
read_db ./results/sky130hd/vsdbabysoc/base/2_floorplan.odb
read_sdc ./results/sky130hd/vsdbabysoc/base/2_floorplan.sdc
source ./platforms/sky130hd/setRC.tcl
```

**Figure 1 — VSDBabySoC Floorplan**

The floorplan shows the core area, the DAC macro, and the power distribution stripes in the upper metal layers.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/6ec9700e-ca8d-48a9-8b0a-eac5a27be658" />


**Figure 2 — Endpoint Slack Chart**

The endpoint slack chart shows the distribution of setup slack at the floorplan stage. The report also identifies 17 unconstrained pins, which require review to determine whether their lack of timing constraints is intentional.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/d655c95e-1c7b-4b2d-a39b-c4c302101c51" />


### 5.2 Placement

Placement positions standard cells in legal locations while considering timing, congestion, and wire length.

Placement-stage parasitics were estimated using:

```tcl
estimate_parasitics -placement
```

The standard-cell locations, DAC macro, and VDD net were inspected using the OpenROAD GUI Inspector.

**Figure 3 — Placement View with VDD Net Selected**

The Inspector identifies VDD as a special power net with 35,710 instance terminals (ITerms). This shows that the net connects to a large number of instance terminals across the design.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/9fbad348-968b-4ccc-bc31-7ca1b9cd51c4" />


**Figure 4 — Macro Region and PDN Stripes**

The zoomed placement view shows the macro region and power distribution stripes across different metal layers.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/c568bf82-3f60-4eb8-babe-35fc70cec33f" />


**Figure 5 — Standard-Cell Placement**

The highly zoomed layout shows standard cells arranged in rows, with horizontal power rails and vertical PDN straps.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/6858db0c-d148-40e6-b19a-67fd8307939e" />


### 5.3 Electrical Design Checks

Maximum capacitance, maximum slew, and maximum fanout checks were reviewed. Hold timing reports were also examined to identify potential minimum-delay issues.

These checks help identify electrical problems that may require correction during physical design optimisation. The final violation status should be established from the relevant reports.

### 5.4 Clock Tree Synthesis

Clock Tree Synthesis constructs the clock distribution network and inserts clock buffers to distribute the clock to sequential cells.

The CTS database, `4_cts.odb`, was opened to inspect the clock network.

**Figure 6 — Post-CTS Layout**

The post-CTS view shows the design after clock-tree construction. The cell-density colour map illustrates the distribution of cells across the core.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/9493da21-2ac0-4ded-bf96-45de6d112128" />


### 5.5 Routing

Routing connects the cells using metal layers and vias. Global routing plans the interconnect paths, while detailed routing determines their final geometry.

Global-routing parasitics were estimated using:

```tcl
estimate_parasitics -global_routing
```

**Figure 7 — Routed VSDBabySoC**

The routed design shows interconnect across the available metal layers, including li1 and met1 through met5, with vias connecting different layers.

<img width="827" height="465" alt="image" src="https://github.com/user-attachments/assets/4429db30-b2c4-441e-892c-6c14d846e415" />


## 6. Post-Route Static Timing Analysis Results

The post-route timing reports were stored in `~/Desktop/postroute_sta/`. The reported worst setup and hold slack values were extracted and compared across 16 PVT corners.

The following shell loop was used:

```bash
cd ~/Desktop/postroute_sta

for f in *.rpt; do
    echo "$f: $(grep -m1 'worst slack max' "$f") | $(grep -m1 'worst slack min' "$f")"
done
```

**Figure 8 — Post-Route Slack Extraction**

The terminal output shows the extracted worst setup and hold slack lines for the timing reports. The corrected loop produced the results after an initial attempt encountered a shell syntax error.
<img width="679" height="541" alt="image" src="https://github.com/user-attachments/assets/1af906fb-6112-4473-afa7-417adc948bf3" />


### 6.1 Complete Timing Results

| Timing Corner | Worst Setup Slack (ns) | Worst Hold Slack (ns) | Hold Status | Setup Status |
|---|---:|---:|---|---|
| `ff_100C_1v65` | 8.15 | 0.25 | MET | MET |
| `ff_100C_1v95` | 9.03 | 0.20 | MET | MET |
| `ff_n40C_1v56` | 6.91 | 0.29 | MET | MET |
| `ff_n40C_1v65` | 7.62 | 0.25 | MET | MET |
| `ff_n40C_1v76` | 8.24 | 0.23 | MET | MET |
| `ff_n40C_1v95` | 8.94 | 0.19 | MET | MET |
| `ss_100C_1v40` | -2.72 | 0.89 | MET | VIOLATED |
| `ss_100C_1v60` | 2.10 | 0.63 | MET | MET |
| `ss_n40C_1v28` | -29.05 | 1.68 | MET | VIOLATED |
| `ss_n40C_1v35` | -15.57 | 1.25 | MET | VIOLATED |
| `ss_n40C_1v40` | -9.98 | 1.09 | MET | VIOLATED |
| `ss_n40C_1v44` | -6.71 | 0.94 | MET | VIOLATED |
| `ss_n40C_1v60` | 0.43 | 0.63 | MET | MET |
| `ss_n40C_1v76` | 3.73 | 0.48 | MET | MET |
| `tt_025C_1v80` | 7.13 | 0.31 | MET | MET |
| `tt_100C_1v80` | 7.26 | 0.31 | MET | MET |

### 6.2 Summary of Results

| Metric | Result |
|---|---|
| Total timing corners | 16 |
| Setup corners met | 11 |
| Setup-violating corners | 5 |
| Hold corners met | 16 |
| Hold-violating corners | 0 |
| Worst setup slack | -29.05 ns |
| Minimum hold slack | 0.19 ns |
| Maximum hold slack | 1.68 ns |

## 7. Setup and Hold Timing Analysis

### 7.1 Hold Timing Analysis

Hold timing checks minimum-delay paths. Data must remain stable for the required interval after the active clock edge.

The reported hold slack is positive across all 16 corners.

| Metric | Result |
|---|---|
| Minimum hold slack | 0.19 ns |
| Corner with minimum hold slack | `ff_n40C_1v95` |
| Maximum hold slack | 1.68 ns |
| Corner with maximum hold slack | `ss_n40C_1v28` |
| Overall reported hold status | MET |

The minimum hold slack occurs at a fast-process, high-voltage corner. The maximum occurs at `ss_n40C_1v28`, where slower data-path behaviour can improve hold margin while making setup timing more difficult.

**Figure 9 — Hold Slack Across 16 PVT Corners**

The hold slack comparison shows positive values at all 16 corners, ranging from 0.19 ns to 1.68 ns.

<img width="856" height="462" alt="image" src="https://github.com/user-attachments/assets/ab9c6b10-66f7-4a1f-a99d-85b1142d67e6" />


### 7.2 Setup Timing Analysis

Setup timing checks maximum-delay paths. Data must arrive early enough to satisfy the capturing register's setup requirement.

Five corners show negative setup slack.

| Timing Corner | Worst Setup Slack (ns) |
|---|---:|
| `ss_n40C_1v28` | -29.05 |
| `ss_n40C_1v35` | -15.57 |
| `ss_n40C_1v40` | -9.98 |
| `ss_n40C_1v44` | -6.71 |
| `ss_100C_1v40` | -2.72 |

The most severe violation occurs at `ss_n40C_1v28`. At -40°C, the setup slack improves as the voltage rises from 1.28 V to 1.44 V. At 1.60 V, the reported setup slack becomes positive at 0.43 ns.

**Figure 10 — Setup Slack Across 16 PVT Corners**

The setup slack comparison highlights the five violating corners and the worst reported setup slack of -29.05 ns at `ss_n40C_1v28`.

<img width="856" height="462" alt="image" src="https://github.com/user-attachments/assets/a7390271-71d6-4602-940e-fae61a890380" />


## 8. Worst Setup Slack Analysis

Worst setup slack identifies the most critical maximum-delay timing result in the analysed corner.

| Metric | Result |
|---|---|
| Worst setup slack | -29.05 ns |
| Critical corner | `ss_n40C_1v28` |
| Process | SS |
| Temperature | -40°C |
| Voltage | 1.28 V |
| Timing status | VIOLATED |

The worst setup slack occurs at the slow-process, low-voltage corner. It indicates that at least one maximum-delay timing path fails the required timing constraint under this condition.

The critical paths should be inspected to determine whether cell delay, interconnect delay, fanout, clock constraints, or another timing factor contributes to the violation.

## 9. Hold Slack Analysis

The reported hold slack ranges from 0.19 ns to 1.68 ns.

| Metric | Result |
|---|---|
| Minimum hold slack | 0.19 ns |
| Minimum-slack corner | `ff_n40C_1v95` |
| Maximum hold slack | 1.68 ns |
| Maximum-slack corner | `ss_n40C_1v28` |
| Reported hold violations | 0 |

The positive reported hold slack indicates that the analysed minimum-delay paths satisfy the reported hold requirements. However, the smallest margin should still be checked against the project's sign-off requirements.

## 10. Timing Comparisons and Physical Design Observations

### 10.1 Floorplan and Power Distribution

The floorplan shows the core, DAC macro, and PDN stripes. The placement inspection identifies VDD as a special power net connected to 35,710 instance terminals.

### 10.2 Standard-Cell Placement

The standard cells are arranged in rows between horizontal power rails, with vertical PDN straps crossing the rows. The macro region and power distribution structures were also examined at higher zoom.

### 10.3 Clock Tree and Routing

The post-CTS layout shows the design after clock-tree construction. The routed layout displays interconnect across the available metal layers and vias.

### 10.4 Multi-Corner Timing Comparison

The results show that:

- All six FF corners meet setup timing.
- Both TT corners meet setup timing.
- Five SS corners violate setup timing.
- All 16 corners report positive hold slack.
- The worst setup slack is -29.05 ns at `ss_n40C_1v28`.
- The smallest hold margin is 0.19 ns at `ff_n40C_1v95`.

The supplied results contain worst setup and hold slack values. TNS values were not provided, so no TNS result is claimed in this report.

## 11. Observations

The following observations are based on the physical-design and timing results:

- Floorplanning, placement, CTS, and routing were inspected using OpenROAD.
- The DAC macro and power distribution network were examined.
- VDD was identified as a special power net with 35,710 instance terminals.
- The floorplan-stage report identified 17 unconstrained pins requiring review.
- Post-route timing analysis covered 16 PVT corners.
- All 16 corners reported positive worst hold slack.
- The minimum hold slack was 0.19 ns.
- Five corners showed setup timing violations.
- The worst setup slack was -29.05 ns at `ss_n40C_1v28`.
- Setup slack improved at the listed -40°C SS corners as voltage increased.
- Setup and hold timing must be evaluated separately because the two checks are affected differently by path delay.
- Further critical-path investigation and timing optimisation are required before sign-off.

## 12. Possible Improvements

### 12.1 Investigate Critical Setup Paths

Inspect the worst setup paths at the violating corners. Determine whether the delay is dominated by cell delay, interconnect delay, fanout, or timing constraints.

### 12.2 Optimise Timing

Depending on the timing reports, possible improvements include cell sizing, cell swapping, buffer insertion, logic restructuring, and placement optimisation.

### 12.3 Review Timing Constraints

Verify the clock period, clock uncertainty, input and output delays, and any false-path or multicycle-path constraints. Constraint changes should reflect the actual design requirements rather than hide violations.

### 12.4 Review Unconstrained Pins

Investigate the 17 unconstrained pins reported at the floorplan stage. Apply appropriate constraints or tie-off logic where required by the design.

### 12.5 Re-run Post-Route STA

After optimisation, regenerate the relevant physical design results and repeat timing analysis across all required corners. Use detailed-routing parasitics when required by the sign-off methodology.

## 13. Conclusion

The physical design flow of VSDBabySoC was examined using OpenROAD-flow-scripts and the SkyWater SKY130HD technology. Floorplanning, placement, Clock Tree Synthesis, and routing were inspected through the OpenROAD GUI.

Post-route Static Timing Analysis was compared across 16 PVT corners. Hold slack was positive at all 16 corners, with a minimum of 0.19 ns.

Setup timing was violated at five corners. The worst setup slack was -29.05 ns at `ss_n40C_1v28`, identifying the slow-process, low-voltage condition as the most critical setup case in the supplied results.

The analysis demonstrates the importance of multi-corner STA for evaluating timing behaviour under different operating conditions. Critical-path investigation, timing-constraint review, and appropriate physical optimisation are required before the design can be considered setup-clean across all required corners.

## 14. Tools and Technologies

- OpenROAD-flow-scripts for physical design.
- OpenROAD GUI for layout inspection.
- OpenSTA for static timing analysis.
- SkyWater SKY130HD process design kit.
- `setRC.tcl` for routing-layer RC configuration.
- GitHub Codespaces for the development environment.
- noVNC for remote graphical access.
- Linux shell and Tcl commands for design inspection.
- Corner-specific post-route timing reports for setup and hold comparison.
