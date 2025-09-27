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


Hierarchical vs Flat Synthesis

Hierarchical synthesis → Preserves module boundaries

Flattened synthesis → Direct gate-level design without hierarchy
