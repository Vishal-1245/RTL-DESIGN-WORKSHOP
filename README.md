# RTL Design Workshop - Complete Guide

## 🎓 Overview
This comprehensive RTL (Register Transfer Level) Design Workshop covers fundamental to advanced concepts in digital hardware design using Verilog. The workshop is structured as a 5-day progression, each building upon the previous day's knowledge, culminating in a complete understanding of modern RTL design practices.

This workshop is ideal for:
- Students learning digital design and HDL
- Engineers transitioning into RTL design
- Professionals seeking to refresh their fundamentals
- Anyone interested in FPGA and ASIC design

## 📚 Workshop Structure

The workshop is organized into five progressive days, each focusing on specific RTL design concepts:

### [DAY 1: MUX (Multiplexer) Design Fundamentals](./DAY_1/README.md)
**Focus:** Combinatorial logic basics and multiplexer design patterns

**Key Topics:**
- Multiplexer fundamentals and 2x1 MUX design
- RTL design patterns (conditional, if-else, case statements)
- Simulation using Icarus Verilog and GTKWave
- Basic synthesis with Yosys
- Waveform analysis and verification

**Files:** `good_mux_both.png`, `good_mux_gtk.png`

**Skills Gained:**
- Understanding combinatorial logic
- Writing testbenches
- Analyzing simulation waveforms
- Basic synthesis and gate-level review

---

### [DAY 2: Sequential Logic & Asynchronous Design](./DAY_2/README.md)
**Focus:** Sequential logic, flip-flops, and hierarchical design

**Key Topics:**
- D Flip-Flop (DFF) design and behavior
- Asynchronous vs synchronous reset mechanisms
- Metastability and race conditions
- Hierarchical module design and instantiation
- Netlist flattening vs hierarchy preservation

**Files:** `asynchronous.png`, `dff_asy_gtk.png`, `dff_syncres.png`, `flatten_netlist.png`, `multiple_modules.png`, `sub_module1.png`

**Skills Gained:**
- Designing reliable sequential circuits
- Understanding clock-domain issues
- Creating reusable, hierarchical components
- Making trade-offs between hierarchy and optimization

---

### [DAY 3: DFF with Constants & Logic Optimization](./DAY_3/README.md)
**Focus:** Advanced DFF designs and synthesis optimization techniques

**Key Topics:**
- DFF behavior with constant inputs
- Constant propagation in synthesis
- Logic optimization and redundancy elimination
- Boolean algebra simplification
- Counter design and applications
- Synthesis tool optimization levels

**Files:** `deff_const2_gtk.png`, `deff_const2_yosys.png`, `deff_const3_gtk.png`, `dff_const3_yosys.png`, `dff_const_gtk.png`, `dff_const_yosys.png`, `good_counter.png`, `opt_check.png`, `opt_check2.png`, `opt_check3.png`

**Skills Gained:**
- Understanding constant propagation
- Writing optimization-friendly code
- Analyzing synthesis results
- Designing efficient counters and state machines
- Comparing before/after optimization metrics

---

### [DAY 4: Blocking vs Non-Blocking & Advanced Multiplexers](./DAY_4/README.md)
**Focus:** Critical assignment semantics and advanced MUX designs

**Key Topics:**
- Blocking (`=`) vs Non-Blocking (`<=`) assignments
- Why non-blocking is essential for sequential logic
- Simulation vs synthesis mismatches
- Advanced multiplexer implementations
- Ternary operator and case-based multiplexers
- Design verification and waveform analysis

**Files:** `bad_mux_gtk.png`, `bad_mux_yosys.png`, `blocking_caveat_gtk.png`, `blocking_caveat_yosys.png`, `ternary_operator_mux_rtl.png`, `ternary_operator_mux_yoys_rtl.png`

**Skills Gained:**
- Mastering assignment semantics
- Avoiding simulation-synthesis mismatches
- Writing race-condition-free code
- Designing efficient multiplexers
- Debugging timing issues

---

### [DAY 5: Case Statements, Completeness & Design Control Flow](./DAY_5/README.md)
**Focus:** Advanced control structures and design completeness

**Key Topics:**
- Case statement syntax and semantics
- Complete vs incomplete designs
- Latch inference from incomplete assignments
- Generate blocks for parameterized designs
- Demultiplexers and decoders
- State machines and control flow
- Gate-level simulation (GLS) vs behavioral simulation

**Files:** `bad_case_gls.png`, `bad_case_gtk.png`, `bad_case_yosys.png`, `comp_case_yosys.png`, `demux_case_gtk.png`, `demux_generate_gtk.png`, `incomp_case_gtk.png`, `incomp_case_yosys.png`, `incomp_if.v_gtk.png`, `incomp_if2_gtk.png`, `incomp_if2_yosys.png`, `incomp_if_yosys.png`, `mux_generate_gtk.png`, `mux_generate_yosys.png`, `partial_case_assign_yosys.png`, `rca.v_gtk.png`

**Skills Gained:**
- Writing complete, latch-free designs
- Using generate blocks effectively
- Implementing state machines
- Detecting unintended synthesis artifacts
- Performing comprehensive design verification

---

## 🛠️ Essential Tools & Commands

### HDL Simulation
**Icarus Verilog** - Open-source Verilog compiler and simulator
```bash
# Compile Verilog design and testbench
iverilog -o output_file design.v testbench.v

# Run simulation and generate waveform dump
vvp output_file

# View waveforms in GTKWave
gtkwave dump.vcd
```

### Waveform Viewer
**GTKWave** - Visual waveform analyzer for simulation results
```bash
# Open waveform file
gtkwave simulation.vcd
```

### Hardware Synthesis
**Yosys** - Open-source synthesis tool
```bash
# Basic synthesis
yosys -p "read_verilog design.v; synth_ice40 -json netlist.json"

# Synthesis with optimization
yosys -p "read_verilog design.v; opt -full; synth_ice40"

# View synthesis result
yosys -p "read_verilog design.v; synth_ice40; show"

# Generate statistics and reports
yosys -p "read_verilog design.v; synth_ice40; stat"
```

### Flow: From RTL to Gate-Level
```bash
# 1. Run behavioral simulation
iverilog -o rtl_sim design.v tb.v
vvp rtl_sim
gtkwave rtl_simulation.vcd

# 2. Synthesize to gate-level
yosys -p "read_verilog design.v; synth_ice40 -json netlist.json; show"

# 3. Run gate-level simulation (GLS)
iverilog -o gls_sim netlist.v tb.v
vvp gls_sim
gtkwave gls_simulation.vcd

# 4. Compare results
# (manually compare waveforms for timing/logic correctness)
```

## 📋 Key Design Patterns Quick Reference

### Combinatorial MUX
```verilog
// Ternary operator (preferred)
assign y = (sel) ? a : b;

// Always block
always @(*)
  y = (sel) ? a : b;

// Case statement
always @(*)
  case(sel)
    1'b0: y = a;
    1'b1: y = b;
    default: y = 'x;
  endcase
```

### Sequential Logic (DFF)
```verilog
// Non-blocking assignment (REQUIRED)
always @(posedge clk or negedge reset_n)
  if (~reset_n)
    q <= 1'b0;  // Asynchronous reset
  else
    q <= d;     // Store input on clock edge
```

### Complete Case Statement
```verilog
always @(*)
  case(sel)
    2'b00: y = a;
    2'b01: y = b;
    2'b10: y = c;
    2'b11: y = d;
    default: y = 'x;  // Always include default
  endcase
```

### Parameterized Design with Generate
```verilog
parameter WIDTH = 8;

genvar i;
generate
  for (i = 0; i < WIDTH; i = i + 1) begin : LOOP
    assign out[i] = in[i] & enable;
  end
endgenerate
```

## ✅ Best Practices Summary

### Assignment Semantics
- ✓ Use **blocking assignment (`=`)** in combinatorial logic (`always @(*)`)
- ✓ Use **non-blocking assignment (`<=`)** in sequential logic (`always @(posedge clk)`)
- ✓ Use **`assign`** for simple combinatorial expressions
- ✓ Never mix blocking and non-blocking in the same block

### Control Flow
- ✓ Always include **default clause** in case statements
- ✓ Ensure **all conditional paths** assign all outputs (prevent latches)
- ✓ Use **generate blocks** for parameterized, scalable designs
- ✓ Keep designs **simple and readable**

### Design Quality
- ✓ **Simulate** before synthesizing (catch logic errors early)
- ✓ **Analyze waveforms** to verify correctness
- ✓ **Compare RTL and GLS** simulations (catch synthesis artifacts)
- ✓ **Check synthesis reports** for inferred latches and timing
- ✓ **Use constraints** to specify clock frequency and paths
- ✓ **Test edge cases** and corner conditions

### Naming & Documentation
- ✓ Use **clear, descriptive names** for signals and modules
- ✓ Follow **consistent naming conventions** (e.g., `_n` for active-low)
- ✓ Document **module interfaces** with comments
- ✓ Add comments for **non-obvious logic**
- ✓ Maintain **consistent indentation**

---

## 🔄 Recommended Learning Flow

1. **Start with DAY 1** to understand combinatorial logic and basic MUX design
2. **Progress to DAY 2** to learn sequential elements and module hierarchy
3. **Continue to DAY 3** for optimization and efficient design patterns
4. **Study DAY 4** to master assignment semantics (critical concept!)
5. **Finish with DAY 5** for advanced control structures and complete designs

**Recommended Approach:**
- Read through each day's README for conceptual understanding
- Examine the provided waveform images (`.png` files) to visualize behavior
- Recreate the designs using the commands provided
- Write testbenches to verify behavior
- Compare behavioral and synthesis results
- Modify examples to deepen understanding

---

## 🎯 Common Pitfalls to Avoid

| Pitfall | Consequence | Solution |
|---------|-------------|----------|
| Using blocking in sequential blocks | Simulation-synthesis mismatch | Use non-blocking `<=` |
| Incomplete case statements | Unintended latches inferred | Add default clause |
| Missing else in if-else chain | Latches for undefined cases | Complete all branches |
| Mixing blocking and non-blocking | Race conditions | Keep one pattern per block |
| Not verifying with testbench | Undetected errors | Always write and run tests |
| Only running behavioral simulation | Synthesis artifacts missed | Also run GLS |
| Ignoring synthesis reports | Optimization issues unknown | Review Yosys reports |
| Poor naming conventions | Code hard to understand/maintain | Use clear, consistent names |

---

## 📊 Progression Summary

```
DAY 1: Basic MUX
    ↓
DAY 2: Add Sequential Logic (DFFs)
    ↓
DAY 3: Optimize & Refine
    ↓
DAY 4: Master Assignment Semantics
    ↓
DAY 5: Advanced Control & Completeness
    ↓
✓ Complete RTL Design Foundation
```

---

## 🚀 Next Steps After Workshop

After completing this workshop, you'll be ready to:
- Design small to medium-sized digital circuits
- Work with FPGA design flows
- Understand ASIC design fundamentals
- Read and review others' RTL code
- Optimize designs for area, power, and timing

**Advanced Topics to Explore:**
- Clock domain crossing (CDC)
- Formal verification
- Timing analysis and constraints
- Low-power design techniques
- High-level synthesis (HLS)
- Advanced state machines

---

## 📞 Getting Help

For each topic:
1. Read the day's README file thoroughly
2. Review the provided waveform images
3. Run the suggested commands
4. Experiment with design variations
5. Check synthesis reports and waveforms
6. Consult Verilog language references if needed

---

## 📄 File Structure

```
RTL_Design_Workshop/
├── README.md (this file)
├── DAY_1/
│   ├── README.md (DAY 1 guide)
│   ├── good_mux_both.png
│   └── good_mux_gtk.png
├── DAY_2/
│   ├── README.md (DAY 2 guide)
│   ├── asynchronous.png
│   ├── dff_asy_gtk.png
│   └── ... (other files)
├── DAY_3/
│   ├── README.md (DAY 3 guide)
│   └── ... (optimization examples)
├── DAY_4/
│   ├── README.md (DAY 4 guide)
│   └── ... (assignment and MUX examples)
└── DAY_5/
    ├── README.md (DAY 5 guide)
    └── ... (case statements and completeness examples)
```

---

## 📖 Verilog Resources

- **IEEE 1364** - Verilog standard
- **Open-source tools**: Icarus Verilog, Yosys, GTKWave (all free!)
- **Online simulators**: EDA Playground (for quick testing)
- **Books**: "Verilog HDL" by Samir Palnitkar, "RTL Modeling with Verilog" by Cummings

---

## 🎓 Learning Outcomes

Upon completing this workshop, you will be able to:

✓ Design and verify combinatorial logic circuits  
✓ Implement sequential logic with proper synchronization  
✓ Write synthesizable, race-condition-free RTL code  
✓ Use simulation and synthesis tools effectively  
✓ Analyze and optimize digital designs  
✓ Understand simulation-synthesis relationships  
✓ Create parameterized, reusable components  
✓ Verify designs with comprehensive testbenches  
✓ Interpret synthesis reports and waveforms  
✓ Follow industry-standard design practices  

---

## 📝 License & Attribution

This workshop is designed for educational purposes. All tools used (Icarus Verilog, Yosys, GTKWave) are open-source and freely available.

---

**Happy Learning! Begin with [DAY 1: MUX Design Fundamentals](./DAY_1/README.md)** 🎯
