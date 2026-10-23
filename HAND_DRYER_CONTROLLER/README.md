# Digital Hand-Dryer Auto-Off Controller — RTL to GDSII

**Technology:** SkyWater Sky130 HD · **Physical Design:** OpenLane · **HDL:** Verilog

## Project Overview

This project implements a **digital hand-dryer auto-off controller** in Verilog and documents its implementation through an open-source RTL-to-GDSII flow.

When a hand is detected, the controller enables the dryer and clears its timer. When the hand is removed, a 7-bit counter counts clock cycles and switches the output off when the configured timeout (`DRY_TIME = 100`) is reached. An active-high asynchronous reset immediately disables the output.

The project demonstrates the major stages of a digital VLSI implementation flow using the **SkyWater Sky130 HD standard-cell library** and OpenLane, including RTL simulation, synthesis, gate-level simulation, floorplanning, placement, clock-tree synthesis, routing, parasitic extraction, static timing analysis, GDSII generation, and physical verification.

> **Project scope:** This is an educational digital-design and physical-design project. It is not a safety-certified controller for direct connection to a real hand dryer.
> ---

## Final GDSII Layout

The final physical layout of the Digital Hand-Dryer Auto-Off Controller, generated using the SkyWater Sky130 HD technology and the OpenLane RTL-to-GDSII flow.

![Final GDSII Layout](https://github.com/user-attachments/assets/9b19d21c-01d7-4825-aa78-8a18f538a38c)

---


### Project Snapshot

| Parameter | Value |
|---|---|
| Design | Hand-dryer auto-off controller |
| Inputs | `clk`, `reset`, `presence` |
| Output | `dryer_enable` |
| Timeout parameter | `DRY_TIME = 100` |
| Counter width | 7 bits |
| Clock period | 20 ns (50 MHz) |
| Standard-cell library | `sky130_fd_sc_hd` |
| OpenLane run tag | `util30` |
| Final synthesis | 46 cells, 446.68 µm² |
| RTL / GLS verification | 7/7 checks passed in each simulation |

The reported results correspond to the documented `util30` run and its associated evidence.

---

## 1. Design Specification

### 1.1 Input and Output Ports

| Port | Direction | Width | Description |
|---|---|---:|---|
| `clk` | Input | 1 | System clock; 20 ns period in the physical-design flow |
| `reset` | Input | 1 | Active-high asynchronous reset |
| `presence` | Input | 1 | Level-sensitive hand-presence signal |
| `dryer_enable` | Output | 1 | Dryer ON/OFF control signal |

**Parameter:** `DRY_TIME = 100`

The counter width is calculated as `CNT_W = $clog2(DRY_TIME)`, giving 7 bits for the default parameter. This calculation assumes `DRY_TIME > 1`.

### 1.2 Operating Behaviour

| Condition | Controller action |
|---|---|
| `reset = 1` | Clear the counter and immediately set `dryer_enable = 0` |
| `presence = 1` | Clear the counter and set `dryer_enable = 1` |
| `presence = 0` and `dryer_enable = 1` | Count clock cycles; turn the output off when `counter == DRY_TIME - 1` |
| Otherwise | Keep the counter cleared |

**Important:** The timeout begins when the hand is removed, not when the hand is first detected.

---

## 2. Tools and Environment

| Purpose | Tool / Technology |
|---|---|
| RTL and gate-level simulation | Icarus Verilog 10.1 (`iverilog`, `vvp`) |
| Waveform inspection | GTKWave |
| Logic synthesis | Yosys 0.9 |
| Floorplanning, placement, CTS and routing | OpenROAD 0.9.0 |
| Clock-tree synthesis | TritonCTS 2.0 |
| Global and detailed routing | FastRoute, TritonRoute |
| Layout and DRC | Magic, KLayout |
| LVS and circuit checks | Magic extraction, Netgen LVS, CVC |
| Static timing analysis | OpenSTA (`/usr/bin/sta`) |
| PDK and standard-cell library | `sky130A`, `sky130_fd_sc_hd` |
| Platform | Ubuntu virtual machine in VirtualBox; OpenLane running inside Docker |

### 2.1 OpenLane Configuration

The main settings in `config.tcl` are:

```tcl
set ::env(DESIGN_NAME)       hand_dryer_controller
set ::env(VERILOG_FILES)     [glob $::env(DESIGN_DIR)/src/*.v]
set ::env(CLOCK_PORT)        clk
set ::env(CLOCK_PERIOD)      20
set ::env(FP_CORE_UTIL)      30
set ::env(PL_TARGET_DENSITY) 0.40
set ::env(GLB_RT_ADJUSTMENT) 0.15
```

**Run tag:** `util30`

### 2.2 RTL-to-GDSII Flow

```text
RTL Design (Verilog)
        |
        v
RTL Simulation (Icarus Verilog)
        |
        v
Logic Synthesis (Yosys + ABC)
        |
        +------> Gate-Level Simulation (GLS)
        |
        v
Floorplanning, IO Placement and Power Distribution
        |
        v
Placement and Legalization
        |
        v
Clock Tree Synthesis (TritonCTS)
        |
        v
Global and Detailed Routing
        |
        v
Parasitic Extraction (SPEF)
        |
        v
Multi-Corner Static Timing Analysis
        |
        v
GDSII Generation
        |
        v
DRC, LVS, Antenna, CVC and XOR Checks
```

### 2.3 Project Folder Structure

```text
FINAL_SUBMISSION/
├── RTL/                    hand_dryer_controller.v
├── TESTBENCH/              hand_dryer_controller_tb.v
├── RTL_SIMULATION/         simulation_results.txt, hand_dryer.vcd
├── SYNTHESIS/              NETLIST/, REPORTS/, SCHEMATIC/
├── GLS/                    GLS testbench, logs, VCD and helper files
├── LAYOUT/                 routed DEF
├── ROUTING/                routed SPEF
├── GDS/                    Magic GDS and KLayout GDS
├── REPORTS/                DRC and routing reports
├── STA/                    timing scripts, constraints and logs
├── STA_EVIDENCE/           ideal/propagated-clock STA evidence
├── FLOW_EVIDENCE/          OpenLane logs, reports and results
├── SCREENSHOTS/            project figures and evidence
├── config.tcl
└── VERIFICATION_STATUS.txt
```

---

## 3. Running the Flow

> **Environment note:** The archive does not establish a universally reproducible launch command. The following commands document the original environment and may require path or environment adjustments on another machine.

### 3.1 Run the Complete Flow

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -design hand_dryer_controller -tag util30
```

The `docker` command shown above is a custom alias or wrapper in the original environment, not a standard Docker command available on every system.

### 3.2 Run Stage by Stage

```bash
./flow.tcl -interactive
```

Inside the OpenLane interactive shell:

```tcl
package require openlane 0.9
prep -design hand_dryer_controller -tag util30
run_synthesis
run_floorplan
run_placement
run_cts
run_routing
run_magic
run_magic_spice_export
run_magic_drc
run_lvs
run_antenna_check
```

The documented run outputs are stored under:

```text
designs/hand_dryer_controller/runs/util30/
```

This run directory contains the corresponding `results/`, `reports/`, and `logs/` subdirectories.

---

## 4. Stage 1 — RTL Design

**Source file:** `RTL/hand_dryer_controller.v`

The controller uses an `always @(posedge clk or posedge reset)` block, a 7-bit counter and one output register, giving **8 flip-flops in total**.

**Figure 1 — RTL source (`hand_dryer_controller.v`)**

![RTL source]
<img width="1920" height="918" alt="rtl_source" src="https://github.com/user-attachments/assets/0cdc7eb8-cc70-4f86-83a8-2dedd56325ff" />

---

## 5. Stage 2 — RTL Simulation

**Testbench:** `TESTBENCH/hand_dryer_controller_tb.v`

The self-checking testbench uses a 10 ns clock and verifies seven functional conditions.

| Check | Verification | Result |
|---:|---|---|
| 1 | Output remains OFF during reset | PASS |
| 2 | Output turns ON after a one-cycle `presence` pulse | PASS |
| 3 | Output eventually turns OFF after waiting 105 cycles | PASS |
| 4 | Output remains ON 50 cycles after hand removal | PASS |
| 5 | Output turns OFF after a further 60 cycles | PASS |
| 6 | Reset is applied while the output is ON | PASS |
| 7 | Asynchronous reset clears the output without a clock edge | PASS |

### Simulation Commands

```bash
iverilog -o rtl_sim RTL/hand_dryer_controller.v TESTBENCH/hand_dryer_controller_tb.v
vvp rtl_sim
gtkwave hand_dryer.vcd
```

**Figure 2 — RTL simulation results.** The preserved `RTL_SIMULATION/simulation_results.txt` records checks 1–7 as PASS and ends with `TEST DONE: ALL PASSED`.

![RTL simulation pass]
<img width="1920" height="918" alt="rtl_simulation_pass" src="https://github.com/user-attachments/assets/b83b0e4b-46d9-45f3-9ce6-d43471b27bd7" />

**Figure 3 — RTL waveform in GTKWave (`clk`, `reset`, `presence`, `dryer_enable`)**

![RTL waveform]
<img width="1920" height="918" alt="rtl_waveform" src="https://github.com/user-attachments/assets/261ea08d-181f-4431-a740-9f4e2e72841e" />

**Figure 4 — RTL waveform showing timeout and subsequent activation.** The output falls at timeout and rises again on the next `presence` pulse.

![RTL waveform timeout]
<img width="1837" height="892" alt="GTKWAVE" src="https://github.com/user-attachments/assets/a8be7207-4da2-4ea8-81bf-60ee63f61ffb" />

---

## 6. Stage 3 — Logic Synthesis (Yosys)

The RTL is synthesized and mapped to Sky130 HD standard cells using Yosys and ABC with the area-oriented synthesis strategy (`SYNTH_STRATEGY = AREA 0`).

### Commands

```bash
run_synthesis
cat reports/synthesis/1-yosys_4.stat.rpt
grep -i -E "number of cells|chip area" reports/synthesis/*yosys_4*stat*
```

### Final Synthesis Results

The final `util30` synthesis report records:

- **Total standard cells:** 46
- **Flip-flops:** 8 × `dfrtp_2` (7 counter bits and 1 output register)
- **Reported cell area:** 446.6784 µm², approximately 446.68 µm²
- **Yosys CHECK:** No reported problems

| Cell | Count | Cell | Count |
|---|---:|---|---:|
| `dfrtp_2` | 8 | `o221a_2` | 3 |
| `inv_2` | 11 | `and3_2` | 2 |
| `buf_1` | 10 | `nor2_2` | 2 |
| `o21ai_2` | 2 | `or4_2` | 2 |
| `and2b_2` | 1 | `and3b_2` | 1 |
| `and4b_2` | 1 | `o211a_2` | 1 |
| `o311a_2` | 1 | `o31a_2` | 1 |

**Figure 5 — Synthesis report files.** The numeric results are taken from `SYNTHESIS/REPORTS/1-yosys_4.stat.rpt` and the matching final `util30` report in `FLOW_EVIDENCE/`.

![Synthesis statistics]
<img width="1920" height="918" alt="synthesis_statistics" src="https://github.com/user-attachments/assets/ac734be7-3376-4dbd-bc6c-26253a1e943e" />

**Figure 6 — Terminal synthesis statistics.** This screenshot also contains an earlier backup result. The final design is the report showing 46 cells and 446.6784 µm²; the earlier 133-cell / 1377.57 µm² result is not the final implementation.

![Synthesised stats]
<img width="877" height="737" alt="synthesised stats" src="https://github.com/user-attachments/assets/9836a781-d5a4-4acf-8239-6c3eaa84e297" />

**Figure 7 — Gate-level netlist generated by Yosys (`hand_dryer_controller_gate_level_netlist.v`)**

![Gate-level netlist]
<img width="1920" height="918" alt="gate_level_netlist" src="https://github.com/user-attachments/assets/b0ac1cc8-ee5d-4a03-901d-c01fd8c8986b" />

An identical copy of this image is also stored at the submission root as `gate_level_netlist.png`.

**Figure 8 — Gate-level schematic.** Graphviz rendering of `gate_level.dot`; the full-size `gate_level.png` and `gate_level.svg` are stored in `SYNTHESIS/SCHEMATIC/`.

![Gate-level schematic]
<img width="911" height="9142" alt="gate_level" src="https://github.com/user-attachments/assets/73b16fa0-ad75-4e1c-a764-3f4005f3cd07" />

**Figure 9 — Earlier-iteration synthesis log.** This log shows the previous generic-gate implementation with 265 cells and 33 DFFs. It is included only for historical comparison and does **not** describe the final netlist.

![Earlier synthesis log]
<img width="947" height="902" alt="sysnthesised netlsit" src="https://github.com/user-attachments/assets/08d68307-2676-4949-a5e1-9f6578d283be" />

---

## 7. Stage 4 — Gate-Level Simulation (GLS)

The synthesized netlist is tested using the same self-checking testbench and the Sky130 **functional** cell models. This simulation uses unit-delay models without SDF back-annotation.

### Commands

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o hand_dryer_gls.vvp \
         GLS/gls_compat.v <sky130 cell models> \
         SYNTHESIS/hand_dryer_controller_gate_level_netlist.v \
         GLS/hand_dryer_controller_gls_tb.v

vvp hand_dryer_gls.vvp
gtkwave hand_dryer.vcd
```

Replace `<sky130 cell models>` with the functional Verilog models from the installed `sky130_fd_sc_hd` PDK. Include the relevant model directories as required by the installation. `GLS/compile_functional.log` records the include error encountered when the model paths are missing.

The other `.log` files in `GLS/` document earlier unsuccessful compile attempts; the successful simulation log is `GLS/simulation.log`.

**Result:** `GLS/simulation.log` records checks 1–7 as PASS and ends with `TEST DONE: ALL PASSED`.

This establishes the tested functional behaviour of the synthesized logic. It is **not** post-layout timing verification because SDF back-annotation is not used.

**Figure 10 — GLS waveform.** Internal netlist signals and `counter[0..6]` count toward the timeout before `dryer_enable` falls.

![GLS waveform]
<img width="1920" height="918" alt="gls_waveform" src="https://github.com/user-attachments/assets/99957ed6-dc98-40c0-afea-0b00514521b0" />

---

## 8. Stage 5 — Floorplanning

Floorplanning defines the die and core, places the IO pins, inserts tap and end-cap cells, and constructs the power distribution network.

### Commands

```bash
run_floorplan
cat reports/floorplan/3-verilog2def.die_area.rpt
cat reports/floorplan/3-verilog2def.core_area.rpt
```

| Item | Result |
|---|---|
| Die dimensions | 49.625 × 60.345 µm |
| Core dimensions | 38.18 × 38.08 µm (approximately 1453.9 µm²) |
| Core rows | 14 |
| Target core utilization | 30% |
| Fixed cells | 44: 16 tap cells and 28 end-cap cells |
| Power distribution | OpenLane warned that the core was too small for a full grid and used a minimized grid |

**Figure 11 — Floorplan view showing the core, tap/end-cap cells and IO pins**

![Floorplan]
<img width="2048" height="2048" alt="floorplan" src="https://github.com/user-attachments/assets/1180b0f4-062f-46d2-81dc-cbb73658333f" />

**Figure 12 — Floorplan DEF render (`hand_dryer_controller.floorplan.def`).** This shows the same stage as Figure 11 with a different view and zoom.

![Floorplan DEF]
<img width="2048" height="2048" alt="hand_dryer_controller floorplan def" src="https://github.com/user-attachments/assets/c3bde7ab-d41d-45d3-b1fb-a5be80b23f4b" />

---

## 9. Stage 6 — Placement

The placement stage performs global placement using RePlAce, followed by timing repair, legalization and detailed placement using OpenDP.

### Commands

```bash
run_placement
cat logs/placement/11-opendp.log
```

| Stage | Instances | Utilization | Notes |
|---|---:|---:|---|
| Global placement and resizer | 93 | 33% | HPWL reduced from 916.4 to 875.4 µm (−4.5%); zero displacement reported |
| Post-CTS timing repair | 96 | 35% | HPWL reduced from 1022.0 to 972.7 µm (−4.8%); maximum displacement 10.1 µm |

**Figure 13 — Placement statistics after global placement and resizer (93 instances, 33% utilization)**

![Placement stats]
<img width="822" height="652" alt="placement stats" src="https://github.com/user-attachments/assets/c2cc1d10-8c7a-4790-bd31-16da8d90ad03" />

**Figure 14 — Placement statistics after CTS timing repair (96 instances, 35% utilization)**

![Placement statistics]
<img width="856" height="606" alt="placement statistics" src="https://github.com/user-attachments/assets/c55aa5ba-58a7-4052-a264-8d39d4308971" />

**Figure 15 — Placed design (`hand_dryer_controller.placement.def`)**

![Placement DEF]
<img width="2048" height="2048" alt="hand_dryer_controller placement def" src="https://github.com/user-attachments/assets/9efe7641-6468-43c2-be9a-0c398f31e4f0" />

---

## 10. Stage 7 — Clock Tree Synthesis (CTS)

TritonCTS builds the clock tree for `clk`, which drives **8 flip-flop sinks**. CTS was the longest stage in the documented flow, taking approximately 2 minutes and 40 seconds.

### Commands

```bash
run_cts
cat logs/cts/13-cts.log
```

**Figure 16 — TritonCTS 2.0 startup and characterization log**

![CTS stats]
<img width="785" height="322" alt="cts stats" src="https://github.com/user-attachments/assets/46fdc5a5-309a-4208-a7e3-ac0d3fa67aa6" />

**Figure 17 — Layout after CTS (`hand_dryer_controller.cts.def`)**

![CTS DEF]
<img width="2048" height="2048" alt="hand_dryer_controller cts def" src="https://github.com/user-attachments/assets/18f8ff54-fb7f-4e2e-9ffa-5ea58cb5716f" />

---

## 11. Stage 8 — Routing and Parasitic Extraction

The routing stage uses FastRoute for global routing and TritonRoute for detailed routing, followed by extraction of routed interconnect parasitics into SPEF format.

### Commands

```bash
run_routing
grep "completing 100%" logs/routing/21-tritonRoute.log | tail -3
cat reports/final_summary_report.csv
```

| Item | Result |
|---|---|
| Final TritonRoute violations | **0** |
| Total routed wire length | 1068 µm |
| Vias | 340 |
| Nets | 55 |
| Antenna violations | 0 pins, 0 nets |
| Main outputs | Routed DEF in `LAYOUT/`; SPEF in `ROUTING/` |

**Figure 18 — Routing statistics and final summary.** The evidence shows the TritonRoute violation count, GDS file locations and reported final summary values.

![Routing stats]
<img width="1856" height="907" alt="Routing stats" src="https://github.com/user-attachments/assets/a3c2c82f-1562-41a8-ab07-54168b9175b7" />

### 11.1 Post-Routing Results

After routing, the design layout is used for parasitic extraction and downstream verification.

```bash
ls results/routing/
head results/routing/hand_dryer_controller.spef
cat reports/routing/41-antenna.rpt | tail -5
```

| Item | Result |
|---|---|
| Routed DEF | `LAYOUT/hand_dryer_controller.def` — 227 components and 55 nets |
| Extracted parasitics | `ROUTING/hand_dryer_controller.spef` |
| TritonRoute DRC | 0 violations; report is empty |
| Antenna check | 0 pins and 0 nets violated |
| Next stages | SPEF-based STA, GDS generation and physical checks |

**Figure 19 — Fully routed design (`hand_dryer_controller.def`)**

![Routed DEF]
<img width="2048" height="2048" alt="hand_dryer_controller def" src="https://github.com/user-attachments/assets/3358500e-4f57-4eda-a513-9410565b2835" />

---

## 12. Stage 9 — Static Timing Analysis (STA)

Static timing analysis was performed using OpenSTA on the post-CTS design with the **routed SPEF**. The analysis covers 16 Sky130 HD Liberty corners in two clock modes:

- **Practical clock:** propagated clock, including clock-tree delay.
- **Ideal clock:** ideal clock for comparison.

The 16 corners comprise 6 fast-fast (`ff`), 8 slow-slow (`ss`) and 2 typical (`tt`) corners.

### 12.1 Timing Constraints

The constraints are defined in `STA/hd.sdc`.

| Constraint | Value |
|---|---|
| Clock period | 20 ns |
| Clock uncertainty | 0.25 ns |
| Input delay on `presence` | 4 ns |
| Output delay on `dryer_enable` | 4 ns |

### 12.2 Commands

```bash
bash STA_EVIDENCE/sta_all.sh
bash STA_EVIDENCE/rerun_sta_verified.sh
sta -no_splash STA/tt_025C_1v80.tcl
grep -H -E 'worst slack|^tns' STA_RERUN/practical/*.log
```

The scripts perform the 16-corner analysis and the ideal/propagated-clock reruns.

### 12.3 Results

All slack values below are in nanoseconds. Each pair is **ideal / practical**.

| Corner | Setup slack (ns) | Hold slack (ns) | TNS |
|---|---:|---:|---:|
| `ff_100C_1v65` | 15.22 / 15.16 | 0.18 / 0.19 | 0.00 |
| `ff_100C_1v95` | 15.34 / 15.27 | 0.09 / 0.10 | 0.00 |
| `ff_n40C_1v56` | 15.12 / 15.07 | 0.22 / 0.24 | 0.00 |
| `ff_n40C_1v65` | 15.19 / 15.14 | 0.17 / 0.19 | 0.00 |
| `ff_n40C_1v76` | 15.26 / 15.21 | 0.12 / 0.14 | 0.00 |
| `ff_n40C_1v95` | 15.34 / 15.29 | **0.06 / 0.08** | 0.00 |
| `ss_100C_1v40` | 13.81 / 13.81 | 1.24 / 1.28 | 0.00 |
| `ss_100C_1v60` | 14.40 / 14.38 | 0.80 / 0.82 | 0.00 |
| `ss_n40C_1v28` | **5.67 / 5.35** | 2.54 / 2.53 | 0.00 |
| `ss_n40C_1v35` | 10.11 / 9.84 | 1.81 / 1.86 | 0.00 |
| `ss_n40C_1v40` | 12.03 / 11.79 | 1.47 / 1.52 | 0.00 |
| `ss_n40C_1v44` | 13.13 / 12.94 | 1.27 / 1.30 | 0.00 |
| `ss_n40C_1v60` | 14.24 / 14.30 | 0.77 / 0.81 | 0.00 |
| `ss_n40C_1v76` | 14.68 / 14.66 | 0.53 / 0.54 | 0.00 |
| `tt_025C_1v80` | 15.07 / 15.05 | 0.26 / 0.27 | 0.00 |
| `tt_100C_1v80` | 15.06 / 15.04 | 0.28 / 0.29 | 0.00 |

**Key timing results**

- **Worst setup slack:** +5.35 ns practical / +5.67 ns ideal at `ss_n40C_1v28`, on a flop-to-flop path through the counter logic.
- **Worst hold slack:** +0.08 ns practical / +0.06 ns ideal at `ff_n40C_1v95`.
- All 32 included rerun logs have corresponding zero-exit-code files. Every reported setup and hold slack is positive, with TNS = 0.00.

> **Evidence consistency note:** `VERIFICATION_STATUS.txt` lists different hold-slack values (+2.53 ns / +2.54 ns). These conflict with the individual `STA_RERUN` logs, which show minimum hold slack of +0.08 ns practical and +0.06 ns ideal. The individual rerun logs are the more specific evidence; the summary file should be corrected.

**Figure 20 — 16-corner STA summary (setup and hold, ideal/practical)**

![16 corner STA results]
<img width="886" height="876" alt="16 corner sta results" src="https://github.com/user-attachments/assets/e3cb49f0-7d6c-4049-b2f9-4edecfef4ff1" />

**Figure 21 — Ideal-clock and practical-clock results compared**

![STA ideal vs practical]
<img width="627" height="610" alt="sta results ideal vs practical" src="https://github.com/user-attachments/assets/64d3f3e5-c9cb-411e-962a-6073e9db91f7" />

`SCREENSHOTS/sta results udeal vs practical.png` is a byte-identical duplicate of Figure 21, with a typo in the filename.

**Figure 22 — Worst setup/hold slack reported for each corner in the ideal-clock run**

![STA worst slack]
<img width="882" height="752" alt="sta_worst_slack" src="https://github.com/user-attachments/assets/baa1f3ef-4bc8-44bb-84f2-e2d5b0ee0fda" />

**Figure 23 — TT corner summary (25 °C, 1.80 V): setup +15.05 ns, hold +0.27 ns, TNS 0.00**

![STA TT corner]
<img width="1920" height="1080" alt="sta tt croner" src="https://github.com/user-attachments/assets/4ddcd0df-587e-446c-bfa3-3166238359fd" />

**Figure 24 — TT-corner timing paths.** The setup path runs from `_76_` to the `dryer_enable` output; the hold path runs from `_76_` to `_77_`.

![STA timing path]
<img width="856" height="827" alt="sta timing path" src="https://github.com/user-attachments/assets/d79d707d-b314-4e87-893c-0adb9de0039e" />

**Figure 25 — Timing-path evidence with a misleading filename.** The original evidence caption identifies a TT-corner path, while the filename refers to the SS, −40 °C, 1.28 V corner. For the actual worst setup result (+5.35 ns practical), refer to `STA_EVIDENCE/STA_RERUN/practical/ss_n40C_1v28.log` rather than relying on the screenshot filename.

![STA worst setup path]
<img width="1920" height="918" alt="STA_SS_N40C_1V28_Worst_Setup_Path png" src="https://github.com/user-attachments/assets/626f9b1c-bcf2-4fe8-bf61-a6aa2aba5672" />

---

## 13. Stage 10 — GDSII Generation and Physical Verification

Magic generates the final GDSII layout. KLayout produces a second GDS, and an XOR comparison is used to examine the differences between the two outputs.

### Commands

```bash
run_magic
run_magic_spice_export
run_magic_drc
run_lvs
run_antenna_check

find . -name "*.gds"
klayout results/magic/hand_dryer_controller.gds
magic -T $PDK_ROOT/sky130A/libs.tech/magic/sky130A.tech
```

### Physical Verification Summary

| Check | Result |
|---|---|
| Magic DRC | **0 violations** (`39-magic.drc`: COUNT 0) |
| LVS | **0 errors** — “Circuits match uniquely” |
| Antenna check | 0 pins and 0 nets violated |
| CVC | 0 errors |
| TritonRoute DRC | 0 violations; report is empty |
| Magic-versus-KLayout GDS XOR | **11,201 differences reported**; unresolved |

**GDS files documented in the submission:**

- `GDS/hand_dryer_controller_magic.gds` — 295,836 bytes
- `GDS/hand_dryer_controller_klayout.gds` — 205,710 bytes

**Figure 26 — Final layout rendered from the Magic GDS**

![Magic GDS]
<img width="2048" height="2048" alt="hand_dryer_controller magic_gds" src="https://github.com/user-attachments/assets/9b19d21c-01d7-4825-aa78-8a18f538a38c" />

**Figure 27 — Final layout rendered from the KLayout GDS**

![KLayout GDS]!
[Uploading hand_dryer_controller.gds.png…]()

**Figure 28 — XOR GDS render showing differences between the Magic and KLayout GDS files**

![XOR GDS]
<img width="2048" height="2048" alt="hand_dryer_controller xor gds" src="https://github.com/user-attachments/assets/59e4c116-2d15-4830-804a-71ecc9ae60af" />

**Figure 29 — Magic GUI showing the final layout with `DRC=0`**

![Magic DRC]
<img width="955" height="905" alt="magic_drc_layout" src="https://github.com/user-attachments/assets/45c1f33d-d7ff-45c9-8cf5-418a28486922" />

---

## 14. Final Results

| Category | Result |
|---|---|
| Design | Sequential controller with 3 inputs, 1 output, `DRY_TIME = 100`, 7-bit counter and asynchronous reset |
| Functional verification | RTL simulation: 7/7 PASS; GLS: 7/7 PASS |
| Synthesis | 46 cells, including 8 flip-flops; area 446.68 µm² |
| Floorplan | Die 49.625 × 60.345 µm; core 38.18 × 38.08 µm; target utilization 30% |
| Tap/end-cap cells | 44 |
| Placement | 33% utilization after placement; 35% after CTS timing repair |
| CTS | Clock net `clk`, 8 sinks |
| Routing | 0 TritonRoute violations; 1068 µm wire length; 340 vias; 55 nets |
| Timing | All 16 corners analysed in ideal and practical modes; worst setup +5.35 ns practical; worst hold +0.08 ns practical; TNS 0.00 |
| Physical checks | Magic DRC 0; LVS 0 errors; antenna 0; CVC 0 |
| Runtime | Approximately 4 min 48 s for the full OpenLane flow; CTS took approximately 2 min 40 s |

### Key Takeaways

1. **Counter optimization reduced the design size.** Reducing the counter from 32 bits to 7 bits reduced the reported synthesis result from 133 cells to 46 cells and area from 1377.57 µm² to 446.68 µm²—approximately a 68% area reduction.
2. **Setup timing passed in the reported corners.** The lowest practical setup slack was +5.35 ns at `ss_n40C_1v28`.
3. **Hold timing has the smallest margin.** The lowest practical hold slack was +0.08 ns at `ff_n40C_1v95`; the ideal-clock result was +0.06 ns.
4. **Clock-mode differences were small for this design.** The reported ideal and propagated-clock slack values differed by only a few hundredths of a nanosecond in the tightest checks.
5. **Physical verification results must be interpreted separately.** Magic DRC and LVS report successful results, but the Magic-versus-KLayout XOR differences remain unresolved.

---

## 15. Notes and Limitations

- These results are **not claimed as complete industry sign-off**.
- The KLayout XOR comparison reported 11,201 differences between the Magic and KLayout GDS files. The `klayout_violations` field is `-1` in `final_summary_report.csv`; treat this as unavailable or invalid unless the report definition establishes otherwise. The XOR result remains unresolved, so the two GDS files should **not** be described as equivalent.
- DRC and LVS results reported here were obtained through the documented Magic-based flow.
- OpenSTA warned that `sky130_fd_sc_hd__tapvpwrvgnd_1` was missing from the synthesized netlist and created a black box for `PHY_28`. Liberty `default_fanout_load` warnings were also reported.
- GLS uses functional unit-delay models without SDF back-annotation. It verifies tested logic behaviour, not post-layout timing.
- The core is small, so OpenLane used a minimized power grid.
- The results describe the documented `util30` run and depend on its configuration, PDK, libraries, scripts and tool versions.
- The controller is an educational demonstration and is not certified for safety-critical or mains-powered appliance control.

---

## Author

| Detail | Information |
|---|---|
| Name | Vadla Sai Nikhil |
| Branch | Electronics and Communication Engineering (ECE) |
| University | Anurag University |
| Year | 3rd Year |
