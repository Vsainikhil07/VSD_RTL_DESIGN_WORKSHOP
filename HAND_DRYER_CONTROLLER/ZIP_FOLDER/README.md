## ZIP Folder Structure

```text
FINAL_SUBMISSION/
├── RTL/                  # Verilog RTL source code
├── TESTBENCH/            # RTL testbench
├── RTL_SIMULATION/       # Simulation logs and waveforms
├── SYNTHESIS/            # Synthesized netlist, reports and schematic
├── GLS/                  # Gate-level simulation files and results
├── LAYOUT/               # Routed DEF layout
├── ROUTING/              # Extracted SPEF parasitics
├── GDS/                  # Final Magic and KLayout GDS files
├── REPORTS/              # DRC and routing reports
├── STA/                  # Timing constraints, scripts and reports
├── STA_EVIDENCE/         # Timing analysis logs and screenshots
├── FLOW_EVIDENCE/        # OpenLane run logs and reports
├── SCREENSHOTS/          # Project screenshots and visual evidence
├── config.tcl            # OpenLane configuration
└── VERIFICATION_STATUS.txt  # Verification summary
```

### Contents Summary

- **RTL & Testbench:** Controller design and functional testbench.
- **Simulation:** RTL and gate-level simulation logs and waveform files.
- **Synthesis:** Standard-cell netlist, synthesis statistics and schematic.
- **Physical Design:** Floorplanning, placement, CTS, routing and layout outputs.
- **Verification:** Timing analysis, DRC, LVS, antenna and circuit-check reports.
- **Evidence:** Screenshots, logs and reports documenting the implementation flow.
- **Configuration:** OpenLane settings used for the SKY130 HD implementation.
