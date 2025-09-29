# week-1-_Vignesh_VSD
report of week-1 progress VSD
##### BEFORE ENTERING: Installation of every required software tools completed successfully.
## DAY-1 - Introduction to Verlog RTL Design and Synthesis.

Week-1 VSD Progress Report

### Author: Vignesh
### Course: VSD – RTL Design & Synthesis

## 📌 Overview

This repository documents my Week-1 progress in the VSD program.
Before starting, I successfully installed all required tools:

Yosys – RTL synthesis tool

Icarus Verilog (iverilog) – Verilog simulator

GTKWave – Waveform viewer

GVim – Text editor for editing Verilog/netlist files

## 📅 Day-1 – Introduction to Verilog RTL Design and Synthesis
🔹 Topics Covered

Introduction to Verilog RTL Design

Basics of Verilog HDL

Writing RTL designs and testbenches

Importance of simulation before synthesis

Simulation Tools

Icarus Verilog (iverilog) for simulation

VCD files (Value Change Dump) generation

GTKWave to visualize signals and waveforms

Hands-on Labs with iverilog & GTKWave

Copied the required liberty file from the prescribed GitHub repository

Simulated a MUX design (good_mux.v) using the provided testbench

Observed output in GTKWave

## Interpreted waveforms for correct functionality

(Insert simulation waveform image here)
➡️ ![MUX Simulation Output](images/mux_simulation.png)

GVim Commands & Netlist Exploration

Opened synthesized netlist using GVim

## Explored design hierarchy, signals, and attributes

(Insert GVim exploration screenshot here)
➡️ ![GVim Netlist View](images/gvim_netlist.png)

🔹 Logic Synthesis Concepts

What is Logic Synthesis?

Process of converting RTL code → Gate-level netlist using standard cell libraries

Slow Cells vs Fast Cells

Slow cells: Higher delay, lower power consumption

Fast cells: Lower delay, higher power consumption

Trade-off between timing and power depending on design requirements

🔹 Yosys Lab Experiments

I performed synthesis using Yosys with the following commands:
```
yosys
# Start yosys tool

read_liberty -lib /home/vicky/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
# Load standard cell library

read_verilog good_mux.v
# Read RTL design file

synth -top good_mux
# Synthesize design with top module 'good_mux'

synth -top module_level
# Synthesize design at specific module level

abc -liberty /home/vicky/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
# Technology mapping using standard cells

show
# Generate schematic of synthesized design

write_verilog /home/vicky/VLSI_Netlist/good_mux_netlist.v
# Write netlist with attributes

write_verilog -noattr good_mux_netlist.v
# Write clean netlist without attributes

!gvim file_name.v
# Open and interact with netlist using GVim
```
## 📅 Day-2 – Introduction to Liberty Files, Cell Characterization & Synthesis Styles

### 🔹 Topics Covered
1. **What is a Liberty (.lib) file?**
   - The Liberty file (`.lib`) is a standard format that describes standard-cell libraries used by synthesis and place-and-route tools.
   - It contains cell names, timing (delay), power, area, and operating conditions (characterized points).
   - Example library used in labs:
     ```
     sky130_fd_sc_hd__tt_025C_1v80.lib
     ```
   - Typical sections/attributes found in a `.lib`:
     - **library name** and metadata
     - **operating conditions**: process corner (p), voltage (v), temperature (t)
     - **cell definitions**: gates, flops, buffers, inverters
     - **timing arcs** and timing tables (input->output delays)
     - **power data**: leakage, dynamic energy
     - **area** for each cell

2. **P, V, T (Process / Voltage / Temperature)**
   - Cells are characterized across combinations of P/V/T (for example: typical-typical corner: `tt`, `0.25µm` or `025C` in file name, `1.80V`).
   - Tools pick the correct timing/power tables based on the chosen operating condition.

3. **Different cell types & variants**
   - **AND gates**: 2-input, 3-input, multi-input — each has different timing/power/area.
   - **Fast cells vs Slow cells**
     - Fast cells: lower delay, usually larger area and higher dynamic power.
     - Slow cells: higher delay, smaller area, lower power.
   - Liberty stores per-cell timing and power so synthesis/technology mapping (e.g., `abc` or `dfflibmap`) can select cells based on constraints.

4. **Hierarchical vs Flattened Synthesis**
   - **Hierarchical synthesis** preserves module boundaries (useful for reuse, incremental flows).
   - **Flattened synthesis** removes hierarchy and produces a flat gate-level netlist — can expose more optimization opportunities at the top level.
   - Use-case:
     - When multiple instances of the same module exist, hierarchical (module-level) synthesis allows `divide-and-conquer` and faster incremental runs.
     - Top-level flattening can enable cross-module optimizations for best global netlist.

5. **Why synth -top (module-level) is used**
   - `synth -top <module>` runs synthesis focusing on a specific top module level.
   - Helps control scope (faster iterations), avoid unnecessarily flattening large designs, and get a better netlist for the intended top-level module.

6. **Flop coding styles & glitches**
   - Studied flop styles (edge-triggered DFFs) with **synchronous** and **asynchronous** reset/set.
   - Relationship between flops and glitches:
     - Combinational logic can produce glitches due to input changes and differing path delays.
     - Using flops as storage elements (registering signals) avoids ripple-through transient glitches by sampling stable values on the clock edge.
     - More combinational depth → higher chance of transient glitches — register boundaries reduce this.

7. **Practical experiments**
   - Synthesized DFF circuits (sync and async reset/set) and observed waveforms for:
     - `dff_asyncres`
     - `dff_async_set`
     - `dff_syncreset` (if present)
   - Took screenshots of waveforms and compared behavior (reset timing, metastability behaviors, etc.).

---

### 🔹 Commands used in the lab & what they do
```bash
# 1) Yosys / synthesis example (module-level)
yosys
read_liberty -lib /home/vicky/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty /home/vicky/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog /home/vicky/VLSI_Netlist/good_mux_netlist.v
write_verilog -noattr good_mux_netlist.v

# 2) Flattening (remove hierarchy for gate-level)
flatten
# Use this when you want a flat gate-level netlist for top-level analysis.

# 3) Mapping DFFs and other cells using liberty
dfflibmap -liberty /home/vicky/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
# dfflibmap maps register constructs to library flops, creating accurate flop instances.

# 4) Open files with gvim (useful for viewing multiple files)
gvim mult_*.v -o
```
## 📅 Day-3 – Logic Optimization

### 🔹 Lab Commands with Detailed Explanations

```bash
# Step 1: List all files for optimization
ls **
```
### Lists all files in the directory recursively.
### Specifically, we look for files generated by previous synth steps:
### - opt_*.v (optimized netlist candidates)
### - opt_check_*.v (for verification/comparison)

# Step 2: Clean and optimize netlist
```
opt_clean -purge
```
### Removes unused logic and nets from the netlist.
### - Purge removes any dangling signals that do not affect outputs.
### - Prepares the netlist for sequential and combinational optimization.
### Ensures that only relevant logic remains for subsequent mapping.

# Step 3: Map D flip-flops to standard library cells
```dfflibmap -liberty /home/vicky/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
### Infers DFFs in the design and maps them to library flip-flop cells.
### - This step converts RTL-level DFFs to actual gate-level cells.
### - Helps Yosys understand which storage elements are needed and which can be optimized away.
### - Constant outputs (from reset/set logic) may not require actual flip-flops if the output is constant.

# Step 4: Open files for inspection and verification
```
gvim mult_*.v -o
gvim dff_const*.v -o
```
### Opens multiple Verilog files in gvim editor side-by-side for easy comparison.
### - 'mult_*.v' contains multiplier examples; helps check bit-level optimizations.
### - 'dff_const*.v' contains constant DFF examples; allows visual verification of inferred or removed flip-flops.

# Step 5: Observe sequential constant propagation
# Example workflow:
### 1) DFF with reset input grounded: output becomes constant 0 → optimizer may remove the DFF cell.
### 2) DFF with set input grounded: output toggles 0/1 → DFF is inferred.
### This shows how Yosys performs sequential constant propagation.
### Screenshots of waveforms are taken for comparison (insert here later).

# Step 6: State Optimization
### Optimizer removes unused FSM states to reduce logic complexity.
### Example: 3-bit counter
### - Case 1: q <= count[0];  => Only LSB is used, other 2 bits logic removed.
### - Case 2: Full 3-bit logic participates in output calculation => entire counter logic preserved.
### This demonstrates how unused output optimization reduces gate count.

# Step 7: Advanced Sequential Optimization Techniques
### - Retiming: Moves DFFs to balance path delays; allows higher clock frequency without changing total combinational delay.
### - Sequential Cloning: Duplicates flip-flops for physical optimization, reducing fanout or improving timing closure.
### These techniques are applied automatically by Yosys if enabled.
### Screenshots of retimed/cloned netlists can be captured for analysis.

# 📅 Day 4 – Logic Optimization

This day covers **Logic Optimization** techniques in Yosys, including both combinational and sequential optimization methods.  
Optimization reduces redundant logic, simplifies designs, improves area, and helps the circuit run efficiently.

---

## 🔹 Types of Logic Optimization

1. **Combinational Logic Optimization**
   - **Constant Propagation** → Replaces logic driven by constants with fixed values.
   - **Boolean Logic Optimization** → Simplifies Boolean expressions (e.g., `A & 1 = A`, `A | 0 = A`).

2. **Sequential Logic Optimization**
   - **Basic**: Sequential Constant Propagation.
   - **Advanced**: Retiming, State Optimization, Sequential Logic Cloning.

---

## 🔹 Sequential Constant Propagation

Example: **D Flip-Flop with Set and Reset inputs**

- **Case 1: Reset DFF (D-input grounded)**  
  → Output becomes constant `0`.

- **Case 2: Set DFF (D-input grounded)**  
  → Output toggles between `0` and `1` because of the set input.  
  Even though the D-input is constant, behavior depends on the set signal.

➡️ Therefore, depending on input conditions, Yosys may optimize out the DFF cell entirely if its output is constant.

---

## 🔹 State Optimization

- Removes **unused states** in sequential circuits.  
- Example:  
  In a 3-bit counter, if output `Q` only depends on `count[0]`, the other two bits (`count[1]` and `count[2]`) are unused → Yosys eliminates unnecessary logic.  
  But if output depends on all three, then full logic is retained.

---

## 🔹 Retiming

- Technique to redistribute registers (DFFs) across the circuit.  
- **Slack** is balanced equally → system works at higher clock speed.  
- ⚠️ Retiming **does not reduce total delay**; it only balances pipeline stages.

---

## 🔹 Cloning (Physical-Aware Optimization)

- Duplicate sequential logic (DFFs) to reduce fanout load.  
- Helps timing closure during physical design.

---

# 🧪 Lab Work

### Step 1: List optimization-related files
```
ls *
```
Step 2: Run Optimization and Cleaning
```
yosys
opt_clean -purge

    opt_clean -purge → removes unused logic gates, redundant wires, and constants.
```
Step 3: Link Technology Library
```
read_liberty -lib ../sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
    Loads standard cell library for mapping.

Step 4: Read Verilog Design (DFF examples)
```
read_verilog dff_const4.v
read_verilog dff_const5.v
```
Step 5: Synthesize with D Flip-Flops
```
synth -top dff_const4
synth -top dff_const5
```
    Yosys may optimize out unused flip-flops if outputs are constants.

Step 6: Example – Counter Optimization

    Case 1: Q = count[0] → Only logic for LSB is kept.

    Case 2: Q depends on all bits → Full counter logic is implemented.
    
# 📅 Day 5 – Control Constructs and Optimization in Synthesis

In RTL design, the way we describe conditional operations directly influences the logic structure created by synthesis tools.  
Statements like **if**, **case**, and **for** don’t just describe functionality—they also guide how multiplexers, decoders, and replicated logic are generated and optimized.  

The synthesis tool then applies techniques like:
- **Constant propagation** → remove logic that never changes.  
- **Dead code elimination** → discard unused branches.  
- **Resource sharing** → minimize redundant hardware.  

---

## 📂 Contents
1. [If Constructs](#if-constructs)  
2. [Case Constructs](#case-constructs)  
3. [For Loop Constructs](#for-loop-constructs)  

---

## 🔹 If Constructs

The **if-else** block represents *priority logic*.  
- Each condition is checked in sequence.  
- The **first true condition wins** and decides the output.  
- Hardware equivalent → **priority multiplexer**.  

### 🔧 Optimization
- If some conditions are always false → tool removes them.  
- Nested chains may be flattened into simpler Boolean logic.  

### ⚠️ Common Pitfall → *Latch Inference*  
If you write:
```
always @(*) begin
  if (i0)
    y = i1;
end
```
    No value for y when i0 is false.

    Tool assumes y must remember its old value → latch created.

📸 [Insert Screenshot: schematic of inferred latch from incomp_if.v]

To avoid this:

    Use else to cover all paths, or

    Assign default values at the start of the always block.

📸 [Insert Screenshot: waveform of latch behavior vs corrected code]
🔹 Case Constructs

A case block describes branching based on a single selector.

    Hardware equivalent → parallel multiplexer or decoder.

    Faster and more balanced than nested if-else chains.

    Ideal for FSMs, decoders, and multi-way muxes.

⚠️ Risks in Case Statements

    Incomplete coverage

        If some selector values aren’t defined → tool infers latches.

        Fix → always include a default.

📸 [Insert Screenshot: schematic showing latch inferred due to missing case]

    Overlapping cases

        Multiple conditions match the same input.

        Becomes a priority case, not parallel.

        May cause mismatch between simulation and synthesis.

📸 [Insert Screenshot: overlapping case statement behavior]

    Partial assignment

        If not all outputs get assigned in every branch → latch inferred.

📸 [Insert Screenshot: partial_case_assign.v example with one output latched]
🔹 For Loop Constructs

Unlike software, Verilog loops are not iterative at runtime.
They are unrolled during synthesis into actual hardware structures.
1. Procedural for loop

    Written inside an always block.

    Used for repetitive combinational operations.

    Synthesizer creates replicated adders, comparators, etc.

Example:
```
always @(*) begin
  sum = 0;
  for (i = 0; i < 4; i = i + 1)
    sum = sum + data[i];
end
```
➡️ Synthesized as a chain/tree of adders.

📸 [Insert Screenshot: schematic showing loop unrolled into adders]
2. Generate for loop

    Used at module level with generate.

    Best for instantiating multiple blocks (structural replication).

Example:
```
genvar i;
generate
  for (i = 0; i < 4; i = i + 1) begin : gen_block
    dff dff_inst (.clk(clk), .d(d[i]), .q(q[i]));
  end
endgenerate
```
➡️ Produces 4 flip-flops in parallel.

📸 [Insert Screenshot: generate loop creating multiple flip-flops]
🔹 Multiplexer and Demultiplexer Examples

    MUX → implements parallel data selection.
    📸 [Insert Screenshot: multiplexer schematic & waveform]

    DEMUX → routes one input to multiple outputs based on select lines.
    📸 [Insert Screenshot: demux schematic & waveform]

    Generate Loops with MUX/DEMUX → allow scalable designs.
    📸 [Insert Screenshot: generate-based multiplexer/decoder]

📝 Key Takeaways

    if → priority logic, prone to latches if not fully specified.

    case → parallel branching, compact but must cover all conditions.

    for → structural replication, unrolled into hardware, not runtime loops.

    Always write complete conditions to avoid latch inference.

