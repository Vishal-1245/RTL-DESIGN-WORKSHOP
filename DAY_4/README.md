# DAY 4: Blocking vs Non-Blocking & Advanced Multiplexer Designs

## Overview
This day focuses on one of the most critical concepts in RTL design: the difference between blocking and non-blocking assignments. Understanding this distinction is essential for writing correct sequential and combinatorial logic. Additionally, this day covers advanced multiplexer designs and their synthesis implications.

## Learning Objectives
- Master blocking vs non-blocking assignments
- Understand why non-blocking is required for sequential logic
- Learn proper usage patterns for combinatorial vs sequential logic
- Explore advanced multiplexer implementations
- Analyze synthesis results and timing behavior
- Avoid common simulation vs synthesis mismatches

## Files in This Folder

### `bad_mux_gtk.png`
- GTKWave simulation of incorrectly designed multiplexer
- Shows erroneous behavior due to improper blocking assignment usage
- Demonstrates simulation results when blocking is used in sequential context

### `bad_mux_yosys.png`
- Yosys synthesis of the bad_mux design
- Shows hardware interpretation of blocking assignment
- Illustrates simulation-synthesis mismatch

### `blocking_caveat_gtk.png`
- GTKWave waveform demonstrating blocking assignment pitfall
- Shows incorrect signal propagation
- Highlights timing issues from blocking assignments in sequential logic

### `blocking_caveat_yosys.png`
- Synthesis result of blocking caveat example
- Demonstrates how synthesizer interprets blocking assignments differently
- Shows the gap between simulation and actual hardware

### `ternary_operator_mux_rtl.png`
- RTL representation of multiplexer using ternary operator (conditional)
- Clean and efficient multiplexer implementation
- Example: `y = (sel) ? input1 : input0`

### `ternary_operator_mux_yoys_rtl.png`
- Yosys synthesis result of ternary operator-based MUX
- Shows efficient gate-level implementation
- Demonstrates optimal hardware generation

## Key Concepts Covered

1. **Blocking Assignment (`=`)**
   - Executes immediately during simulation
   - Intended for combinatorial logic
   - Used in `always @(*)` or `always @(*)` blocks
   - NOT recommended for sequential logic
   - Creates race conditions in sequential contexts

2. **Non-Blocking Assignment (`<=`)**
   - Evaluates RHS before updating LHS
   - Essential for sequential logic (triggered by clock edge)
   - Used in `always @(posedge clk)` blocks
   - Prevents race conditions
   - Matches actual hardware behavior
   - Multiple non-blocking assignments in same block update simultaneously

3. **Simulation vs Synthesis Mismatch**
   - Blocking in sequential context: simulation differs from hardware
   - Synthesis tools interpret behavior differently
   - Real hardware doesn't have "simulation semantics"
   - Proper assignments ensure simulation matches synthesis

4. **Multiplexer Design Patterns**
   - Using ternary operator (conditional)
   - Using if-else statements
   - Using case statements
   - Behavioral and structural approaches
   - Parameterized multiplexers

## Commands & Usage

### Viewing Simulation Waveforms:
```bash
# Display bad_mux simulation
gtkwave bad_mux.vcd

# Display blocking caveat example
gtkwave blocking_caveat.vcd
```

### Running Simulations:
```bash
# Compile and simulate
iverilog -o mux_sim mux.v tb_mux.v
vvp mux_sim

# Dump waveform for analysis
gtkwave dump.vcd
```

### Synthesis with Yosys:
```bash
# Synthesize MUX design
yosys -p "read_verilog mux.v; synth_ice40 -json mux.json; show"

# Analyze resource usage
yosys -p "read_verilog mux.v; synth_ice40; stat"
```

## Design Patterns

**WRONG - Blocking in Sequential Logic:**
```verilog
always @(posedge clk)
  begin
    a = b;      // BLOCKING - WRONG FOR SEQUENTIAL!
    c = a;      // c gets new value of a, not old
  end
  // Simulation: c = new a
  // Hardware: c might be old value (synthesis mismatch)
```

**CORRECT - Non-Blocking in Sequential Logic:**
```verilog
always @(posedge clk)
  begin
    a <= b;     // NON-BLOCKING - CORRECT
    c <= a;     // c gets old value of a
  end
  // Simulation: c = old a
  // Hardware: c = old a (matches!)
```

**CORRECT - Blocking in Combinatorial Logic:**
```verilog
always @(*)
  begin
    y = (sel) ? a : b;  // BLOCKING - CORRECT FOR COMBINATORIAL
  end
```

**Better - Use Ternary Operator:**
```verilog
assign y = (sel) ? a : b;  // Most readable
```

**Multi-bit Multiplexer:**
```verilog
always @(*)
  case(sel)
    2'b00: y = in0;
    2'b01: y = in1;
    2'b10: y = in2;
    2'b11: y = in3;
    default: y = 'x;
  endcase
```

## Common Pitfalls & Solutions

| Pitfall | Problem | Solution |
|---------|---------|----------|
| Blocking in `@(posedge clk)` | Simulation-synthesis mismatch | Use non-blocking `<=` |
| Non-blocking in `@(*)` | Unnecessarily complex | Use blocking `=` or `assign` |
| Mixed assignments | Race conditions | Keep one pattern per block |
| Uninitialized signals | Undefined behavior | Always assign default values |
| Missing `default` in case | Undefined states | Always include default clause |

## Testing & Verification

### Testbench Considerations:
```verilog
// Test all select lines
initial
  for (sel = 0; sel < 4; sel = sel + 1)
    begin
      #10 $display("Select=%b, Output=%b", sel, output);
    end

// Verify correctness
assert (output == expected)
  else $error("Mismatch at time %t", $time);
```

## Best Practices
- ✓ Always use non-blocking in sequential blocks
- ✓ Always use blocking in combinatorial blocks
- ✓ Use `assign` for simple combinatorial expressions
- ✓ Use ternary operator for 2-to-1 multiplexers
- ✓ Use case statements for larger multiplexers
- ✓ Add default cases to prevent latches
- ✓ Verify waveforms match expectations
- ✓ Compare simulation and synthesis results

## Next Steps
Proceed to DAY_5 to explore case statements, completeness, and advanced control flow designs.
