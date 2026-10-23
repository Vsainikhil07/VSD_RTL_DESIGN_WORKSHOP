# Digital Hand-Dryer Auto-Off Controller — RTL to GDSII (Sky130 · OpenLane)

A small sequential controller that switches a hand dryer **ON** when a hand is detected and switches it **OFF** automatically a fixed number of clock cycles after the hand is removed. The design is taken through the complete digital VLSI flow — **RTL → simulation → synthesis → gate-level simulation → floorplan → placement → CTS → routing → parasitic extraction → multi-corner STA → GDSII → DRC / LVS / antenna checks** — using open-source tools and the SkyWater **Sky130 HD** library.

> Place this file at the root of the `FINAL_SUBMISSION/` folder so all image links below resolve.

---

## 1. Design Specification

| Port | Dir | Width | Description |
|---|---|---|---|
| `clk` | in | 1 | System clock (20 ns period in the physical flow = 50 MHz) |
| `reset` | in | 1 | Active-high **asynchronous** reset |
| `presence` | in | 1 | Hand detected (level-sensitive) |
| `dryer_enable` | out | 1 | Dryer ON/OFF control |

**Parameter:** `DRY_TIME = 100` → counter width `CNT_W = $clog2(100) = 7` bits.

**Behaviour (priority order):**

| Condition | Action |
|---|---|
| `reset = 1` | `counter = 0`, `dryer_enable = 0` (immediately, no clock edge needed) |
| `presence = 1` | `counter = 0`, `dryer_enable = 1` (timer is held at zero while the hand is present) |
| `presence = 0` and `dryer_enable = 1` | count up; when `counter == DRY_TIME-1` → `dryer_enable = 0`, `counter = 0` |
| otherwise | `counter = 0` |

So the timeout starts **when the hand is removed**, not when it arrives.

---

## 2. Tools and Environment

| Purpose | Tool |
|---|---|
| RTL / gate-level simulation | Icarus Verilog 10.1 (`iverilog`, `vvp`) |
| Waveforms | GTKWave |
| Synthesis | Yosys 0.9 (inside OpenLane) |
| Floorplan, placement, CTS, routing | OpenROAD 0.9.0 — TritonCTS 2.0, FastRoute, TritonRoute |
| Layout, DRC, extraction | Magic, KLayout |
| LVS / circuit checks | Magic extraction + netgen LVS, CVC |
| Static timing | OpenSTA (`/usr/bin/sta`) |
| PDK / library | `sky130A` · `sky130_fd_sc_hd` |
| Platform | Ubuntu VM (VirtualBox), OpenLane run inside Docker |

**OpenLane configuration (`config.tcl`):**

```tcl
set ::env(DESIGN_NAME)      hand_dryer_controller
set ::env(VERILOG_FILES)    [glob $::env(DESIGN_DIR)/src/*.v]
set ::env(CLOCK_PORT)       clk
set ::env(CLOCK_PERIOD)     20
set ::env(FP_CORE_UTIL)     30
set ::env(PL_TARGET_DENSITY) 0.40
set ::env(GLB_RT_ADJUSTMENT) 0.15
```

Run tag: **`util30`**.

---

## 2.1 Flow at a Glance

```
RTL (Verilog)
   │  iverilog / vvp / gtkwave
   ▼
RTL Simulation ──► Synthesis (Yosys + ABC, AREA strategy)
                      │            └──► Gate-Level Simulation (GLS)
                      ▼
                  Floorplan ► IO placement ► Tap/End-caps ► PDN
                      ▼
                  Placement (global + legalization + resizer)
                      ▼
                  Clock Tree Synthesis (TritonCTS)
                      ▼
                  Routing (FastRoute ► TritonRoute) ► SPEF extraction
                      ▼
                  Multi-corner STA (16 corners × ideal / propagated clock)
                      ▼
                  GDSII (Magic + KLayout) ► DRC · LVS · Antenna · CVC · XOR
```

---

## 2.2 Folder Structure

```
FINAL_SUBMISSION/
├── RTL/                    hand_dryer_controller.v
├── TESTBENCH/              hand_dryer_controller_tb.v
├── RTL_SIMULATION/         simulation_results.txt, hand_dryer.vcd
├── SYNTHESIS/              NETLIST/, REPORTS/, SCHEMATIC/, synthesis_statistics.rpt
├── GLS/                    GLS testbench, netlist simulation log, VCD, helper files
├── LAYOUT/                 routed DEF
├── ROUTING/                routed SPEF
├── GDS/                    Magic GDS + KLayout GDS
├── REPORTS/                Magic DRC, TritonRoute log/DRC
├── STA/                    16-corner scripts + logs, hd.sdc
├── STA_EVIDENCE/           ideal vs propagated reruns, scripts, screenshot
├── FLOW_EVIDENCE/          full OpenLane logs, reports, results (util30)
├── SCREENSHOTS/            all figures used below
├── config.tcl
└── VERIFICATION_STATUS.txt
```

---

## 3. How to Run the Flow (simple commands)

> The zip does not store the exact launch command. These are the standard OpenLane commands that produce the `runs/util30` folder seen in the logs.

**Enter OpenLane and run everything in one shot**

```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane
docker                                   # alias that opens the OpenLane container
./flow.tcl -design hand_dryer_controller -tag util30
```

**Or run stage by stage (interactive)**

```bash
./flow.tcl -interactive
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

All results land in `designs/hand_dryer_controller/runs/util30/` (`results/`, `reports/`, `logs/`).

---

## 4. Stage 1 — RTL Design

**File:** `RTL/hand_dryer_controller.v` — one `always @(posedge clk or posedge reset)` block, a 7-bit counter and one output register (8 flip-flops in total).

**Figure 1 — RTL source (`hand_dryer_controller.v`)**

![RTL source]<img width="1920" height="918" alt="rtl_source" src="https://github.com/user-attachments/assets/af04b261-3346-43f6-9879-d5275d6cb187" />


---

## 5. Stage 2 — RTL Simulation

**Testbench:** `TESTBENCH/hand_dryer_controller_tb.v` — self-checking, 10 ns clock, 7 checks.

| Check | What is verified | Result |
|---|---|---|
| 1 | Output OFF during reset | PASS |
| 2 | ON after a one-cycle `presence` pulse | PASS |
| 3 | OFF after timeout (105 cycles) | PASS |
| 4 | Hand held, then removed: still ON 50 cycles after removal | PASS |
| 5 | OFF after a further 60 cycles | PASS |
| 6 | Running (ON) just before reset | PASS |
| 7 | Asynchronous reset clears output without a clock edge | PASS |

**Commands**

```bash
iverilog -o rtl_sim RTL/hand_dryer_controller.v TESTBENCH/hand_dryer_controller_tb.v
vvp rtl_sim
gtkwave hand_dryer.vcd
```

**Figure 2 — Simulation log: 7/7 checks pass, `TEST DONE: ALL PASSED`**

![RTL simulation pass]<img width="1920" height="918" alt="rtl_simulation_pass" src="https://github.com/user-attachments/assets/9a779340-d342-485b-85fc-bd15d4ffe75b" />


**Figure 3 — RTL waveform in GTKWave (`clk`, `reset`, `presence`, `dryer_enable`)**

![RTL waveform]<img width="1920" height="918" alt="rtl_waveform" src="https://github.com/user-attachments/assets/7b2da810-4248-4361-ab52-5ef107f359db" />


**Figure 4 — RTL waveform, second view of the same VCD: output falls at timeout and rises again on the next `presence` pulse**

![RTL waveform timeout]<img width="1837" height="892" alt="GTKWAVE" src="https://github.com/user-attachments/assets/b43d545b-ed59-48a8-8f14-2e879d1633ab" />


---

## 6. Stage 3 — Logic Synthesis (Yosys)

RTL is mapped to Sky130 standard cells (ABC, `SYNTH_STRATEGY = AREA 0`).

**Commands**

```bash
run_synthesis                                              # inside OpenLane
cat reports/synthesis/1-yosys_4.stat.rpt                   # cell statistics
grep -i -E "number of cells|chip area" reports/synthesis/*yosys_4*stat*
```

**Result (final design):** **46 cells**, **8 × `dfrtp_2`** (7 counter bits + output register), **chip area 446.68 µm²**, 0 problems from Yosys CHECK.

| Cell | Count | Cell | Count |
|---|---|---|---|
| `dfrtp_2` | 8 | `o221a_2` | 3 |
| `inv_2` | 11 | `and3_2` | 2 |
| `buf_1` | 10 | `nor2_2` | 2 |
| `o21ai_2` | 2 | `or4_2` | 2 |
| `and2b_2`, `and3b_2`, `and4b_2`, `o211a_2`, `o311a_2`, `o31a_2` | 1 each | | |

**Figure 5 — Synthesis statistics report (`1-yosys_4.stat.rpt`)**

![Synthesis statistics]<img width="1920" height="918" alt="synthesis_statistics" src="https://github.com/user-attachments/assets/9825dff6-0513-4292-a708-c8ed900fc80b" />


**Figure 6 — Same report in the terminal. The first lines (133 cells, 1377.57 µm²) are from an earlier backup run with the old 32-bit counter; the report below them is the final `util30` run (46 cells, 446.68 µm²)**

![Synthesised stats]<img width="877" height="737" alt="synthesised stats" src="https://github.com/user-attachments/assets/032ab11d-c24e-4521-8e1f-0af2b4b7a8aa" />


**Figure 7 — Gate-level netlist (`hand_dryer_controller_gate_level_netlist.v`) generated by Yosys**

![Gate-level netlist]<img width="1920" height="918" alt="gate_level_netlist" src="https://github.com/user-attachments/assets/41989066-d6dd-4664-a7eb-61afa27948ef" />


*(An identical copy of this picture is also kept at `FINAL_SUBMISSION/gate_level_netlist.png`.)*

**Figure 8 — Gate-level schematic (Graphviz render of `gate_level.dot`; full-size `gate_level.png` / `gate_level.svg` in `SYNTHESIS/SCHEMATIC/`)**

![Gate-level schematic]<img width="911" height="9142" alt="gate_level" src="https://github.com/user-attachments/assets/b94d9762-4ca9-40b7-bae5-7913e52c6600" />


**Figure 9 — Earlier-iteration synthesis log (generic gates, 265 cells, 33 DFFs). This is from the old design before the counter was reduced and is shown only for the before/after comparison — it does *not* describe the final netlist**

![Earlier synthesis log]<img width="947" height="902" alt="sysnthesised netlsit" src="https://github.com/user-attachments/assets/52247161-26ec-492e-a17f-9db306162691" />


---

## 7. Stage 4 — Gate-Level Simulation (GLS)

The same self-checking testbench is run on the synthesized netlist using the Sky130 **functional** cell models (zero/unit delay, no SDF back-annotation).

**Commands**

```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o hand_dryer_gls.vvp \
         GLS/gls_compat.v <sky130 cell models> \
         SYNTHESIS/hand_dryer_controller_gate_level_netlist.v \
         GLS/hand_dryer_controller_gls_tb.v
vvp hand_dryer_gls.vvp
gtkwave hand_dryer.vcd
```

`<sky130 cell models>` = the `sky130_fd_sc_hd` functional Verilog models from your PDK install (add `-I` for the model folders; `GLS/compile_functional.log` shows the include error you get without it). The other `.log` files in `GLS/` are earlier failed compile attempts; the successful run is `GLS/simulation.log`.

**Result:** 7/7 checks PASS, `TEST DONE: ALL PASSED` — gate-level behaviour matches RTL.

**Figure 10 — GLS waveform: internal netlist nets and `counter[0..6]` counting up to the timeout, then `dryer_enable` falling**

![GLS waveform]<img width="1920" height="918" alt="gls_waveform" src="https://github.com/user-attachments/assets/59d1943e-0add-4258-8166-884fa8b07619" />


---

## 8. Stage 5 — Floorplan

Defines die and core, places the IO pins, inserts tap and end-cap cells and builds the power grid.

**Commands**

```bash
run_floorplan
cat reports/floorplan/3-verilog2def.die_area.rpt
cat reports/floorplan/3-verilog2def.core_area.rpt
```

| Item | Value |
|---|---|
| Die | 49.625 × 60.345 µm |
| Core | 38.18 × 38.08 µm (≈ 1453.9 µm²), 14 rows |
| Core utilization (target) | 30 % |
| Fixed cells | 44 (16 tap + 28 end-cap) |
| Power grid | OpenLane warned the core is too small for a full grid and used a minimized grid |

**Figure 11 — Floorplan view (empty core, tap/end-cap cells, IO pins)**

![Floorplan]<img width="2048" height="2048" alt="floorplan" src="https://github.com/user-attachments/assets/557ffa20-f269-4685-a086-0bcf01f3ec5c" />


**Figure 12 — Floorplan DEF render (`hand_dryer_controller.floorplan.def`). Same stage as Figure 11, rendered with a slightly different view/zoom**

![Floorplan DEF]<img width="2048" height="2048" alt="hand_dryer_controller floorplan def" src="https://github.com/user-attachments/assets/e2712cd8-08d9-46a6-a215-178c1008621d" />


---

## 9. Stage 6 — Placement

Global placement (RePlAce) → resizer → detailed placement/legalization (OpenDP).

**Commands**

```bash
run_placement
cat logs/placement/11-opendp.log
```

| After | Instances | Utilization | Notes |
|---|---|---|---|
| Global placement + resizer | 93 | 33 % | HPWL 916.4 → 875.4 µm (−4.5 %), 0 displacement |
| Post-CTS timing repair | 96 | 35 % | HPWL 1022.0 → 972.7 µm (−4.8 %), max displacement 10.1 µm |

**Figure 13 — Placement statistics after global placement/resizer (93 instances, 33 % utilization)**

![Placement stats]<img width="822" height="652" alt="placement stats" src="https://github.com/user-attachments/assets/f324b737-9394-42df-bec7-2e83c7bb912c" />


**Figure 14 — Placement statistics after CTS timing repair (96 instances, 35 % utilization)**

![Placement statistics]<img width="856" height="606" alt="placement statistics" src="https://github.com/user-attachments/assets/89a4fac8-e124-4a0f-99dd-363e2bf8acd8" />


**Figure 15 — Placed design (`hand_dryer_controller.placement.def`)**

![Placement DEF]<img width="2048" height="2048" alt="hand_dryer_controller placement def" src="https://github.com/user-attachments/assets/8d24c25d-5e42-4f56-b321-a39039b6b84e" />


---

## 10. Stage 7 — Clock Tree Synthesis (CTS)

TritonCTS builds a balanced tree for net `clk`, which drives **8 flip-flop sinks**. This was the longest step of the flow (2 min 40 s).

**Commands**

```bash
run_cts
cat logs/cts/13-cts.log
```

**Figure 16 — TritonCTS 2.0 start-up and characterization (log)**

![CTS stats]<img width="785" height="322" alt="cts stats" src="https://github.com/user-attachments/assets/c38c0706-0489-4025-b452-fa982ea7d593" />


**Figure 17 — Layout after CTS (`hand_dryer_controller.cts.def`)**

![CTS DEF]<img width="2048" height="2048" alt="hand_dryer_controller cts def" src="https://github.com/user-attachments/assets/6b2642b8-6ffe-4f13-9bb2-193b925152e2" />


---

## 11. Stage 8 — Routing and Parasitic Extraction

FastRoute (global) → TritonRoute (detailed) → SPEF extraction.

**Commands**

```bash
run_routing
grep "completing 100%" logs/routing/21-tritonRoute.log | tail -3
cat reports/final_summary_report.csv
```

| Item | Value |
|---|---|
| TritonRoute violations (final iteration) | **0** |
| Total wire length / vias | 1068 µm / 340 |
| Nets | 55 |
| Antenna violations | 0 pins, 0 nets |
| Output | routed DEF (`LAYOUT/`), SPEF (`ROUTING/`) |

**Figure 18 — Terminal: TritonRoute “0 violations”, GDS file locations and the final summary report values**

![Routing stats]<img width="1856" height="907" alt="Routing stats" src="https://github.com/user-attachments/assets/17e39867-032c-4a5e-9a63-83546fb8713a" />


---

## 12. Stage 8A — Post-Routing

Once routing is finished the layout is frozen, and everything after this point is extraction and verification on the **routed** design.

**Commands**

```bash
ls results/routing/                              # routed DEF + SPEF
head results/routing/hand_dryer_controller.spef  # extracted parasitics
cat reports/routing/41-antenna.rpt | tail -5     # antenna check
```

| Post-routing item | Result |
|---|---|
| Routed DEF | `LAYOUT/hand_dryer_controller.def` — 227 components, 55 nets |
| Parasitics (SPEF) | `ROUTING/hand_dryer_controller.spef` (RC extracted from the routed wires) |
| TritonRoute DRC | 0 violations, empty report |
| Antenna check | 0 pins, 0 nets violated |
| Next steps | SPEF-based STA (Section 13), then GDS and DRC/LVS (Section 14) |

**Figure 19 — Fully routed layout (`hand_dryer_controller.def`)**

![Routed DEF]<img width="2048" height="2048" alt="hand_dryer_controller def" src="https://github.com/user-attachments/assets/575dad06-f30c-459a-b92d-42670e30b61f" />


---

## 13. Stage 9 — Static Timing Analysis (16 corners)

OpenSTA on the post-CTS netlist with the **routed SPEF**, run for all **16** Sky130 HD liberty corners (6 × `ff`, 8 × `ss`, 2 × `tt`) in two modes:

- **Practical** — `set_propagated_clock` (real clock-tree delay)
- **Ideal** — no propagated clock (comparison only)

**Constraints (`STA/hd.sdc`):** 20 ns clock, 0.25 ns uncertainty, 4 ns input delay on `presence`, 4 ns output delay on `dryer_enable`.

**Commands**

```bash
bash STA_EVIDENCE/sta_all.sh                          # 16 corners
bash STA_EVIDENCE/rerun_sta_verified.sh               # ideal + practical reruns (32 runs)
sta -no_splash STA/tt_025C_1v80.tcl                   # a single corner
grep -H -E 'worst slack|^tns' STA_RERUN/practical/*.log
```

**Results (ns), ideal / practical:**

| Corner | Setup slack | Hold slack | TNS |
|---|---|---|---|
| ff_100C_1v65 | 15.22 / 15.16 | 0.18 / 0.19 | 0.00 |
| ff_100C_1v95 | 15.34 / 15.27 | 0.09 / 0.10 | 0.00 |
| ff_n40C_1v56 | 15.12 / 15.07 | 0.22 / 0.24 | 0.00 |
| ff_n40C_1v65 | 15.19 / 15.14 | 0.17 / 0.19 | 0.00 |
| ff_n40C_1v76 | 15.26 / 15.21 | 0.12 / 0.14 | 0.00 |
| ff_n40C_1v95 | 15.34 / 15.29 | **0.06 / 0.08** | 0.00 |
| ss_100C_1v40 | 13.81 / 13.81 | 1.24 / 1.28 | 0.00 |
| ss_100C_1v60 | 14.40 / 14.38 | 0.80 / 0.82 | 0.00 |
| ss_n40C_1v28 | **5.67 / 5.35** | 2.54 / 2.53 | 0.00 |
| ss_n40C_1v35 | 10.11 / 9.84 | 1.81 / 1.86 | 0.00 |
| ss_n40C_1v40 | 12.03 / 11.79 | 1.47 / 1.52 | 0.00 |
| ss_n40C_1v44 | 13.13 / 12.94 | 1.27 / 1.30 | 0.00 |
| ss_n40C_1v60 | 14.24 / 14.30 | 0.77 / 0.81 | 0.00 |
| ss_n40C_1v76 | 14.68 / 14.66 | 0.53 / 0.54 | 0.00 |
| tt_025C_1v80 | 15.07 / 15.05 | 0.26 / 0.27 | 0.00 |
| tt_100C_1v80 | 15.06 / 15.04 | 0.28 / 0.29 | 0.00 |

- **Worst setup slack:** +5.35 ns (practical) / +5.67 ns (ideal) at `ss_n40C_1v28` — a flop-to-flop path through the counter logic.
- **Worst hold slack:** +0.08 ns (practical) / +0.06 ns (ideal) at `ff_n40C_1v95`.
- All 32 STA runs exited with code 0; every slack is positive and TNS is 0.00.

**Figure 20 — 16-corner summary table (setup and hold, ideal/practical)**

![16 corner STA results]<img width="886" height="876" alt="16 corner sta results" src="https://github.com/user-attachments/assets/e54ec5eb-746d-45a7-a82d-d1ecf3abaa26" />


**Figure 21 — Ideal vs practical results side by side**

![STA ideal vs practical]<img width="627" height="610" alt="sta results ideal vs practical" src="https://github.com/user-attachments/assets/1d05ad49-a574-4a0c-855b-933c4863092f" />


*(`SCREENSHOTS/sta results udeal vs practical.png` is a byte-identical duplicate of Figure 21 with a typo in the file name.)*

**Figure 22 — Output of `sta_all.sh` (setup/hold worst slack for every corner, ideal-clock run)**

![STA worst slack]


**Figure 23 — TT corner (25 °C, 1.80 V) summary: setup +15.05 ns, hold +0.27 ns, TNS 0.00**

![STA TT corner]![Uploading sta tt croner.png…]()


**Figure 24 — TT corner timing paths: setup path `_76_` → `dryer_enable` (output), hold path `_76_` → `_77_`**

![STA timing path]


**Figure 25 — TT 25 °C / 1.80 V worst setup path (slack +15.05 ns). The file is named `STA_SS_N40C_1V28_…` but the content is the TT corner; the real SS −40 °C 1.28 V worst setup path (+5.35 ns) is in `STA_EVIDENCE/STA_RERUN/practical/ss_n40C_1v28.log`**

![STA worst setup path]



---

## 14. Stage 10 — GDSII, Layout Views and Sign-off Checks

Magic streams the final GDSII; KLayout produces a second GDS and the XOR comparison.

**Commands**

```bash
run_magic                         # GDS from the routed design
run_magic_spice_export            # extraction for LVS
run_magic_drc                     # DRC
run_lvs                           # LVS
run_antenna_check                 # antenna
find . -name "*.gds"
klayout results/magic/hand_dryer_controller.gds
magic -T $PDK_ROOT/sky130A/libs.tech/magic/sky130A.tech
```

| Check | Result |
|---|---|
| Magic DRC | **0 violations** (`39-magic.drc`: COUNT 0) |
| LVS | **0 errors** — “Circuits match uniquely” |
| Antenna | 0 pins / 0 nets violated |
| CVC | 0 errors |
| TritonRoute DRC | empty report (0 violations) |
| KLayout XOR (Magic vs KLayout GDS) | **11,201 differences reported** — see Section 16 |

**Files:** `GDS/hand_dryer_controller_magic.gds` (295,836 B), `GDS/hand_dryer_controller_klayout.gds` (205,710 B).

**Figure 26 — Final layout rendered from the Magic GDS**

![Magic GDS]![Uploading hand_dryer_controller.magic_gds.png…]()


**Figure 27 — Final layout rendered from the KLayout GDS**

![KLayout GDS]![Uploading hand_dryer_controller.gds.png…]()


**Figure 28 — XOR GDS render (difference layout between the Magic and KLayout GDS files)**

![XOR GDS](SCREENSHOTS/hand_dryer_controller.xor.gds.png)

**Figure 29 — Magic GUI with the final layout loaded: toolbar shows `DRC=0`**

![Magic DRC](SCREENSHOTS/magic_drc_layout.png)

---

## 15. Final Results — Overview of the Project

| Area | Result |
|---|---|
| Design | 4-port sequential controller, `DRY_TIME = 100`, 7-bit counter, async reset |
| Functional verification | RTL sim 7/7 PASS · GLS 7/7 PASS |
| Synthesis | 46 cells (8 flip-flops), 446.68 µm² (old 32-bit version: 133 cells, 1377.57 µm²) |
| Floorplan | Die 49.625 × 60.345 µm, core 38.18 × 38.08 µm, 30 % utilization, 44 tap/end-cap cells |
| Placement | 33 % utilization after placement, 35 % after CTS repair |
| CTS | Clock net `clk`, 8 sinks |
| Routing | 0 TritonRoute violations, 1068 µm wire, 340 vias, 55 nets |
| Timing (20 ns clock) | All 16 corners × 2 modes met: worst setup +5.35 ns, worst hold +0.08 ns, TNS 0.00 |
| Physical verification | Magic DRC 0 · LVS 0 errors · antenna 0 · CVC 0 |
| Runtime | 4 min 48 s for the full OpenLane flow (CTS = 2 min 40 s) |

**Key take-aways**

1. Reducing the counter from 32 bits to 7 bits cut the design from 133 to 46 cells and the area by about 68 %.
2. Setup timing has large margin (20 ns clock vs ~14 ns worst path); the tightest checks are **hold in the fast corners** (`ff_n40C_1v95`, +0.06 to +0.08 ns).
3. The worst setup corner is `ss_n40C_1v28` (slow-slow, −40 °C, 1.28 V); the worst hold corner is `ff_n40C_1v95`.
4. Practical (propagated-clock) and ideal-clock results differ by only a few hundredths of a nanosecond for this tiny 8-sink clock tree.

---

## 16. Notes and Limitations

- These results are **not claimed as full industry sign-off.**
- **KLayout XOR** reported 11,201 differences between the Magic and KLayout GDS files, and `klayout_violations` is `-1` in `final_summary_report.csv`. DRC and LVS in this flow were run on the Magic layout; the XOR result should be reviewed before claiming the two GDS files are equivalent.
- OpenSTA warned that `sky130_fd_sc_hd__tapvpwrvgnd_1` was missing from the synthesized netlist and created a black box for `PHY_28`; there are also Liberty `default_fanout_load` warnings.
- GLS uses functional (unit-delay) models without SDF back-annotation, so it checks logic equivalence, not post-layout timing.
- The core is very small, so OpenLane used a minimized power grid.
