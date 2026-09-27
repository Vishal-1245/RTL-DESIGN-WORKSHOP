# DAY 5: Case Statements, Completeness & Design Control Flow

## Overview
This day covers advanced case statement usage, understanding completeness in design (complete vs incomplete case and if statements), and how these affect synthesized hardware. This includes practical applications like demultiplexers, counters with generate blocks, and hierarchical design patterns.

## Learning Objectives
- Master case statement syntax and behavior
- Understand complete vs incomplete designs
- Learn how incomplete statements create latches (unintended)
- Explore generate blocks for scalable designs
- Analyze synthesis implications of design choices
- Prevent unintended sequential behavior in combinatorial logic

## Files in This Folder

### `bad_case_gls.png`
- Gate-Level Simulation (GLS) of incorrect case statement
- Shows actual hardware behavior of bad_case design
- Demonstrates the physical consequences of incomplete design

### `bad_case_gtk.png`
- GTKWave simulation of bad_case design
- Behavioral simulation showing incorrect output
- Example of latch inferred from incomplete case

### `bad_case_yosys.png`
- Yosys synthesis result showing latches in bad_case
- Demonstrates how incomplete case creates sequential elements
- Shows unintended flip-flops in combinatorial logic

### `comp_case_yosys.png`
- Synthesis of complete case statement
- Shows pure combinatorial logic (no latches)
- Efficient gate-level implementation

### `demux_case_gtk.png`
- GTKWave of demultiplexer using case statement
- Shows correct demux behavior across all cases
- Complete case ensuring all outputs are defined

### `demux_generate_gtk.png`
- Demultiplexer implemented with generate block
- Shows parameterized, scalable design
- Demonstrates hardware generation for arbitrary widths

### `incomp_case_gtk.png`
- Incomplete case statement behavioral simulation
- Shows undefined behavior when case doesn't cover all values
- Demonstrates potential metastability issues

### `incomp_case_yosys.png`
- Synthesis of incomplete case showing inferred latches
- Hardware interpretation of unspecified cases
- Illustrates synthesis artifact

### `incomp_if.v_gtk.png`
- Incomplete if statement in GTKWave
- Similar issues to incomplete case
- Shows behavioral simulation problems

### `incomp_if2_gtk.png`
- Another incomplete if statement example
- More complex incompleteness scenario
- Demonstrates various manifestations of the issue

### `incomp_if2_yosys.png`
- Synthesis of complex incomplete if statement
- Shows latches and undefined behavior in hardware
- Illustrates synthesis implications

### `incomp_if_yosys.png`
- Simple incomplete if statement synthesis
- Shows basic latch inference
- Reference example for understanding latch creation

### `mux_generate_gtk.png`
- Multiplexer using generate block
- Demonstrates parameterized multiplexer
- Shows scalable design pattern

### `mux_generate_yosys.png`
- Synthesis of generated multiplexer
- Shows how Yosys handles generate blocks
- Illustrates efficient hardware for parameterized designs

### `partial_case_assign_yosys.png`
- Partial case assignment synthesis
- Shows incomplete assignment to multi-bit outputs
- Demonstrates dangers of partial case coverage

### `rca.v_gtk.png`
- Ripple Carry Adder (RCA) simulation in GTKWave
- Shows arithmetic operation implementation
- Example of hierarchical component usage

## Key Concepts Covered

1. **Case Statements**
   - Full syntax and semantics
   - Priority encoding in hardware
   - Default clause importance
   - Different case types: case, casex, casez
   - Case statement vs if-else performance implications

2. **Completeness in Design**
   - Complete case: all possible input values handled
   - Complete if-else: covers all possible conditions
   - Incomplete designs create latches (sequential elements)
   - Unintended latches consume power and area
   - May cause timing violations

3. **Latch Inference**
   - When and why latches are created
   - Latches from incomplete case statements
   - Latches from incomplete if statements
   - Latches from incomplete assignments
   - Using Yosys to detect inferred latches

4. **Generate Blocks**
   - For-loops in hardware generation
   - If-statements in generate blocks
   - Creating parameterized, scalable designs
   - Generate for multiplexers, demultiplexers, adders
   - Generate for memory arrays and bus structures

5. **Common Design Patterns**
   - State machines with case statements
   - Demultiplexers and decoders
   - Arithmetic circuits (adders, counters)
   - Priority encoders
   - Data selectors

## Commands & Usage

### Viewing Simulations:
```bash
# Display GTKWave for case statement
gtkwave bad_case.vcd

# Display complete vs incomplete comparison
gtkwave comp_case.vcd
gtkwave incomp_case.vcd
```

### Running Behavioral Simulation:
```bash
# Compile and run
iverilog -o case_sim case_design.v tb_case.v
vvp case_sim

# Generate waveform dump
gtkwave dump.vcd
```

### Synthesis Analysis:
```bash
# Synthesize and view netlist
yosys -p "read_verilog design.v; synth_ice40; show"

# Detect inferred latches
yosys -p "read_verilog design.v; synth_ice40; stat"

# Generate detailed report
yosys -p "read_verilog design.v; synth_ice40; report"
```

### Generate Block Synthesis:
```bash
# Expand generate blocks
yosys -p "read_verilog -sv design.v; synth_ice40 -json netlist.json"
```

### Gate-Level Simulation (GLS):
```bash
# After synthesis, simulate with netlist
iverilog -o gls_sim netlist.v tb_design.v
vvp gls_sim
gtkwave dump.vcd
```

## Design Patterns

**WRONG - Incomplete Case (Creates Latch):**
```verilog
always @(*)
  case(sel)
    2'b00: y = a;
    2'b01: y = b;
    2'b10: y = c;
    // 2'b11 is NOT defined - latch is inferred!
  endcase
```

**CORRECT - Complete Case (No Latch):**
```verilog
always @(*)
  case(sel)
    2'b00: y = a;
    2'b01: y = b;
    2'b10: y = c;
    2'b11: y = d;  // All cases handled
  endcase
```

**Better - With Default Clause:**
```verilog
always @(*)
  case(sel)
    2'b00: y = a;
    2'b01: y = b;
    2'b10: y = c;
    default: y = 'x;  // Safer, catches unexpected cases
  endcase
```

**WRONG - Incomplete If (Creates Latch):**
```verilog
always @(*)
  begin
    if (sel == 2'b00)
      y = a;
    else if (sel == 2'b01)
      y = b;
    // Missing else for other cases
  end
```

**CORRECT - Complete If:**
```verilog
always @(*)
  begin
    if (sel == 2'b00)
      y = a;
    else if (sel == 2'b01)
      y = b;
    else if (sel == 2'b10)
      y = c;
    else
      y = d;  // All cases covered
  end
```

**Parameterized Multiplexer with Generate:**
```verilog
parameter WIDTH = 8, SEL_WIDTH = 3;

wire [WIDTH-1:0] mux_inputs [0:2**SEL_WIDTH-1];
wire [WIDTH-1:0] mux_out;

always @(*)
  mux_out = mux_inputs[sel];
```

**Demultiplexer with Generate:**
```verilog
parameter WIDTH = 8, SEL_WIDTH = 3;

wire [WIDTH-1:0] din;
wire [WIDTH-1:0] dout [0:2**SEL_WIDTH-1];

genvar i;
generate
  for (i = 0; i < 2**SEL_WIDTH; i = i + 1) begin
    assign dout[i] = (sel == i) ? din : 'z;
  end
endgenerate
```

**State Machine with Case:**
```verilog
parameter IDLE = 2'b00, ACTIVE = 2'b01, DONE = 2'b10;
reg [1:0] state, next_state;

always @(*)
  case(state)
    IDLE:
      if (start)
        next_state = ACTIVE;
      else
        next_state = IDLE;
    ACTIVE:
      if (done)
        next_state = DONE;
      else
        next_state = ACTIVE;
    DONE:
      next_state = IDLE;
    default: next_state = IDLE;
  endcase
```

## Latch Detection Checklist

**Signs of Unintended Latches:**
- ✗ Incomplete case without default
- ✗ Incomplete if-else chain
- ✗ Output not assigned in all conditions
- ✗ Variable used before assignment
- ✗ Feedback loop without explicit storage

**How to Fix:**
- ✓ Add default clause to case
- ✓ Complete if-else chain
- ✓ Assign all outputs in all paths
- ✓ Initialize outputs before conditionals
- ✓ Use non-blocking assignment if storage intended

## Performance Metrics
- **Combinatorial Delay**: Propagation time through logic
- **Setup/Hold Time**: Requirements for flip-flops
- **Power**: Dynamic (switching) and static (leakage)
- **Area**: Number of gates (LUTs, latches, flip-flops)

## Best Practices
- ✓ Always use case statements with default clause
- ✓ Ensure all conditional paths assign all outputs
- ✓ Use generate blocks for parameterized designs
- ✓ Test behavioral simulation and GLS separately
- ✓ Compare simulation and synthesis waveforms
- ✓ Use Yosys synthesis reports to detect latches
- ✓ Document state machines and control flow
- ✓ Verify all states and transitions

## Testing Strategy

### Simulation Layers:
1. **Behavioral Simulation (RTL)**
   - Uses behavioral model
   - Runs quickly
   - Verifies logic correctness

2. **Gate-Level Simulation (GLS)**
   - Uses synthesized netlist
   - Includes real delays
   - Catches synthesis artifacts
   - Verifies actual hardware behavior

3. **Waveform Comparison**
   - Compare RTL vs GLS waveforms
   - Identify simulation-synthesis mismatches
   - Verify timing constraints

## Conclusion
The RTL Design Workshop provides a comprehensive foundation in digital design principles. Understanding completeness, proper assignment types, and scalable design patterns is essential for professional HDL development.

## Next Steps
- Apply these concepts to your own designs
- Explore advanced topics: clock domain crossing, formal verification
- Study industry design standards and guidelines
- Practice on larger, more complex projects
