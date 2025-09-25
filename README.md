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

Hierarchical vs Flat Synthesis

Hierarchical synthesis → Preserves module boundaries

Flattened synthesis → Direct gate-level design without hierarchy
