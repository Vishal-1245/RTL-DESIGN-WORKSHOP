# DAY 2: Sequential Logic & Asynchronous Design

## Overview
This day explores asynchronous design patterns, D Flip-Flop (DFF) implementations, and how to work with multiple modules in RTL design.

## Learning Objectives
- Understand asynchronous signal handling and reset mechanisms
- Learn D Flip-Flop (DFF) design and behavior
- Master multi-module hierarchy and instantiation
- Understand flattening and hierarchical design trade-offs
- Analyze sequential logic in simulation and synthesis

## Files in This Folder

### `asynchronous.png`
- Demonstrates asynchronous reset behavior in flip-flops
- Shows timing diagram of asynchronous vs synchronous reset
- Illustrates race conditions and metastability issues

### `dff_asy_gtk.png`
- GTKWave simulation of DFF with asynchronous reset
- Shows the flip-flop output changing immediately on async reset
- Demonstrates the difference from synchronous reset

### `dff_syncres.png`
- Synchronous reset behavior in D Flip-Flops
- Output changes occur at clock edge after reset is released
- More stable and predictable than asynchronous reset

### `flatten_netlist.png`
- Shows hierarchical vs flattened netlist representation
- Compares the difference in gate-level implementation
- Demonstrates how flattening affects circuit structure

### `multiple_modules.png`
- Illustrates multi-module hierarchical design
- Shows module instantiation and port connectivity
- Demonstrates how to compose larger systems from smaller blocks

### `sub_module1.png`
- Example of a sub-module block diagram
- Shows inputs, outputs, and internal structure
- Useful for understanding hierarchical decomposition

## Key Concepts Covered

1. **D Flip-Flop (DFF)**
   - Basic storage element for sequential circuits
   - Output captures input at clock edge
   - Asynchronous vs synchronous reset behavior
   - Timing specifications (setup, hold, propagation delay)

2. **Asynchronous Design**
   - Immediate response to control signals
   - Potential metastability issues
   - When to use asynchronous signals (reset, control)

3. **Synchronous Design**
   - Changes occur at clock edges
   - Eliminates metastability issues
   - Reliable state transitions

4. **Hierarchical Design**
   - Module instantiation and parameterization
   - Port mapping and connectivity
   - Flattening for optimization or debugging
   - Preserving hierarchy for readability

## Commands & Usage

### Viewing DFF Waveforms:
```bash
# Display simulation in GTKWave
gtkwave dff_simulation.vcd
```

### Running Simulation:
```bash
# With Icarus Verilog
iverilog -o dff_sim dff.v tb_dff.v
vvp dff_sim
```

### Hierarchical Synthesis:
```bash
# Synthesize with hierarchy preservation
yosys -p "read_verilog dff.v; synth_ice40 -json netlist.json"
```

### Flattening Netlist:
```bash
# Flatten hierarchical design
yosys -p "read_verilog design.v; flatten; synth_ice40"
```

## Design Patterns

**Asynchronous Reset:**
```verilog
always @(posedge clk or negedge reset_n)
  if (~reset_n)
    q <= 1'b0;  // Immediate reset
  else
    q <= d;     // Store on clock edge
```

**Synchronous Reset:**
```verilog
always @(posedge clk)
  if (reset_n)
    q <= 1'b0;  // Reset on clock edge
  else
    q <= d;
```

## Best Practices
- Use synchronous reset when possible for reliability
- Keep hierarchical design for maintainability
- Document module interfaces clearly
- Test each module independently before integration
- Use constraints to specify clock frequency

## Next Steps
Proceed to DAY_3 to learn about DFF with constant values and optimization techniques.
