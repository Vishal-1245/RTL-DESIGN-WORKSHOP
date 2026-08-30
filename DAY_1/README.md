# DAY 1: MUX (Multiplexer) Design Fundamentals

## Overview
This day covers the fundamental concepts and design patterns related to Multiplexer (MUX) implementation in RTL (Register Transfer Level) design.

## Learning Objectives
- Understand the basic structure of a multiplexer
- Learn how to implement 2x1 MUX designs
- Explore different design approaches and synthesis results
- Analyze the gate-level implementation of MUX circuits

## Files in This Folder

### `good_mux_both.png`
- Depicts the waveform simulation result of a well-designed 2x1 multiplexer
- Shows both input signals and the selected output
- Demonstrates correct timing and signal propagation

### `good_mux_gtk.png`
- GTKWave (GTK Waveform Viewer) representation of the MUX simulation
- Visual waveform display showing the select line toggling between two inputs
- Useful for understanding the behavioral simulation output

## Key Concepts Covered

1. **Multiplexer Basics**
   - A 2x1 MUX selects one of two inputs based on a select signal
   - Output equation: Y = (S' & I0) | (S & I1)

2. **RTL Design Patterns**
   - Conditional (ternary) operator implementation
   - If-else statement implementation
   - Case statement implementation

3. **Simulation Analysis**
   - Verifying correct behavior with testbenches
   - Waveform analysis using GTKWave

4. **Synthesis Considerations**
   - How the RTL code maps to hardware
   - Optimization during synthesis

## Commands & Usage

### To view waveforms:
```bash
# Open GTKWave to visualize simulation results
gtkwave good_mux_both.vcd
```

### To run simulation:
```bash
# Using Icarus Verilog
iverilog -o output mux.v tb_mux.v
vvp output
```

### To perform synthesis:
```bash
# Using Yosys
yosys -m ghdl
read_verilog mux.v
synth_ice40
write_netlist -json output.json
```

## Design Principles
- Keep designs simple and readable
- Use consistent naming conventions
- Always verify with testbenches
- Analyze synthesis results for optimization opportunities

## Next Steps
After mastering MUX design, move to DAY_2 for asynchronous design patterns and sequential logic.
