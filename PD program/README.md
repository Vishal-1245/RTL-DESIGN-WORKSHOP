# PD Program

This PD Program is a practical learning path focused on ASIC design flow using OpenLane, SkyWater 130nm PDK, and related EDA tools. It introduces the complete digital design flow from environment setup through synthesis, floorplanning, placement, clock tree synthesis, and routing.

## Program Objective

The goal of this program is to help learners understand how a digital design moves from HDL and configuration files to a manufacturable physical implementation. Each module builds on the previous one and focuses on a key step in the ASIC design lifecycle.

## Modules Overview

### Module 1: OpenLane Setup and Design Environment
Focus: Setting up the toolchain, understanding PDKs, and starting the OpenLane workflow.

Topics covered:
- OpenLane installation and environment setup
- SkyWater 130nm PDK integration
- Design directory structure and configuration files
- Initial synthesis and design exploration
- Project understanding with example RTL blocks

Key files:
- `config.tcl_2.png`
- `openlane.png`
- `synthesis.png`
- `synthesis_results.png`

---

### Module 2: Floorplanning and Placement
Focus: Defining chip boundaries, creating placement constraints, and understanding physical layout decisions.

Topics covered:
- Floorplan setup and constraints
- Core area and I/O planning
- Placement strategy and placement quality checks
- Viewing floorplan and placement results
- Reading layout logs and understanding key metrics

Key files:
- `floorplan.png`
- `floorplan_results_def.png`
- `layout.png`
- `placement.png`

---

### Module 3: CMOS Design and Simulation using ngspice
Focus: Understanding transistor-level behavior and analog/digital interaction in the SkyWater flow.

Topics covered:
- CMOS inverter design
- PDK-based circuit simulation
- ngspice workflow and waveform interpretation
- Device behavior under transient analysis
- Design verification at circuit level

Key files:
- `Inverter_sky130A_3.png`
- `Cmosinverter_tcon_3.png`
- `ngspice_1_output_3.png`
- `ngspice_simulation_op.png`

---

### Module 4: Synthesis and Timing Corner Analysis
Focus: Converting RTL to gates and evaluating the design across process corners.

Topics covered:
- Synthesis flow in OpenLane
- Corner-based analysis with sky130 libraries
- Timing characteristics and variation across PVT conditions
- Exploring synthesis output and design quality
- Checking library variants and constraints

Key files:
- `sky130_fast_4.png`
- `sky130_inv_4.png`
- `sky130_typical_4.png`
- `sky140_slow_4.png`
- `Synthesis_4.png`

---

### Module 5: Clock Tree Synthesis, Placement, and Routing
Focus: Completing the backend flow and understanding the final implementation steps.

Topics covered:
- Clock tree synthesis (CTS)
- Placement refinement
- Routing and congestion handling
- Design signoff checks
- Final backend implementation flow

Key files:
- `run_cts_5.png`
- `Run_placement_5.png`
- `Run_routing_5.png`

---

## Tools Used

- OpenLane
- SkyWater PDK (sky130A)
- Magic
- ngspice
- TCL configuration files
- Linux/Unix shell workflow

## Typical Design Flow

1. Configure the design and setup the PDK
2. Run synthesis and review reports
3. Floorplan the chip and define placement constraints
4. Perform placement and optimization
5. Run CTS for clock distribution
6. Execute routing and verify the final layout
7. Inspect reports, logs, and generated outputs

## Learning Outcome

By the end of this program, learners should be able to:
- Set up a digital ASIC design environment
- Understand key OpenLane and Sky130 workflows
- Interpret floorplan, placement, and routing results
- Simulate and analyze transistor-level circuits
- Understand the physical design pipeline from synthesis to layout

## Suggested Progression

Start with Module 1 and move through the modules in order. Each module introduces a core stage in the ASIC implementation flow and is designed to build practical understanding of real backend design tasks.

---

This PD Program is intended as a hands-on introduction to physical design and ASIC implementation fundamentals.
