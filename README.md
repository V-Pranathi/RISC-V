# RISC-V
## Table of Contents  
* [1. RV DAY 1 Introduction to RISC-V ISA and GNU compiler toolchain](#1--rv-day-1-introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
  * RV-D1SK1 - Introduction to RISC-V basic keywords
    * RV_D1SK1_L1_Introduction
    * RV_D1SK1_L2_From Apps To Hardware
    * RV_D1SK1_L3_Detailed Description Of Course Content
  * RV-D1SK2 - Labwork for RISC-V software toolchain
    * RV_D1SK2_L1_C Program To Compute Sum From 1 to N
    * RV_D1SK2_L2_RISCV GCC compile And Disassemble
    * RV_D1SK2_L3_Spike Simulation And Debug
  * RV-D1SK3 - Integer number representation
    * RV_D1SK3_L1_64-bit Number System For Unsigned Numbers
    * RV_D1SK3_L2_64-bit Number System For Signed Numbers
    * RV_D1SK3_L3_Lab For Signed And Unsigned Numbers
* [2. RV Day 2 - Introduction to ABI and basic verification flow](#2--rv-day-2-introduction-to-abi-and-basic-verification-flow)
  * RV-D2SK1 - Application Binary interface (ABI)
    * RV_D2SK1_L1_Introduction To Application Binary Interface
    * RV_D2SK1_L2_Memory Allocation For Double Words
    * RV_D2SK1_L3_Load, Add And Store Instructions With Example
    * RV_D2SK1_L4_Concluding 32-registers And Their Respective ABI Names 
  * RV-D2SK2 - Lab work using ABI function calls
    * RV_D2SK2_L1_Study New Algorithm For Sum 1 to N Using ASM
    * RV_D2SK2_L2_Review ASM Function Call
    * RV_D2SK2_L3_Simulate New C Program With Function Call
  * RV-D2SK3 - Basic verification flow using iverilog
    * RV_D2SK3_L1_Lab To Run C-Program On RISC-V CPU
* [3. RV Day 3 - Digital Logic with TL-Verilog and Makerchip](#3--rv-day-3---digital-logic-with-tl-verilog-and-makerchip)
  * RV-D3SK1 - Combinational logic in TL-Verilog using Makerchip
    * RV_D3SK1_L0_Welcome
    * RV_D3SK1_L1_Introduction To Logic Gates
    * RV_D3SK1_L2_Basic Mux Implementation And Introduction To Makerchip
    * RV_D3SK1_L3_Labs For Combinational Logic
  * RV-D3SK2 - Sequential logic
    * RV_D3SK2_L1_Introduction To Sequential Logic And Counter Lab
    * RV_D3SK2_L2_Sequential Calculator Lab 
  * RV-D3SK3 - Pipelined logic
    * RV_D3SK3_L1_Pipelined Logic And Re-Timing
    * RV_D3SK3_L2_Pipeline Logic Advantages And Demo In Platform
    * RV_D3SK3_L3_Lab On Error Conditions Within Computation Pipeline
    * RV_D3SK3_L4_Lab On 2-Cycle Calculator 
  * RV-D3SK4 - Validity
    * RV_D3SK4_L1_Introduction To Validity And Its Advantages
    * RV_D3SK4_L2_Lab On Validity And Valid When Condition
    * RV_D3SK4_L3_Lab To Compute Total Distance
    * RV_D3SK4_L4_Lab on 2-cycle Calculator with Validity
    * RV_D3SK4_L5_Calulator Single Value Memory Lab
  * RV Day 3 Wrap-up
    * RV_D3SK5_L1_Introduction To Hierarchy Concept
    * RV_D3SK5_L2_Day3_closer 
* [4. RV Day 4 - Basic RISC-V CPU micro-architecture](#4-rv-day-4---basic-risc-v-cpumicro-architecture)
  * RV-D4SK1 - Introduction to Simple RISC-V Micro-architecture
    * RV_D4SK1_L1_Micro-architecture of Single Cycle RISC-V CPU
    * RV_D4SK1_L2_Starting Point Code for RISC-V Labs Part-1
    * RV_D4SK1_L3_Starting Point Code for RISC-V Labs Part-2
  * RV-D4SK2 - Fetch and decode
    * RV_D4SK2_L1_Implementation Plan and Lab for PC
    * RV_D4SK2_L2_Lab For Instruction Fetch Logic
    * RV_D4SK2_L3_Lab For RV Instruction Types IRSBJU Decode Logic
    * RV_D4SK2_L4_Lab For Instruction Immediate Decode Logic For RV-ISBUJ
    * RV_D4SK2_L5_Lab To Decode other Fields of Instructions For RV-ISBUJ
    * RV_D4SK2_L6_Lab To Decode Instruction Field Based on Instr Type RV-ISBUJ
    * RV_D4SK2_L7_Lab To Decode Individual Instruction 
  * RV-D4SK3 - RISC-V control logic
    * RV_D4SK3_L1_Lab For Register File Read Part1 (USE UPDATED SHELL CODE)
    * RV_D4SK3_L2_Lab For Register File Read Part-2
    * RV_D4SK3_L3_Lab For ALU Operations For add/addi
    * RV_D4SK3_L4_Lab For Register File Write
    * RV_D4SK3_L5_Concept of Array And Register File Details
    * RV_D4SK3_L6_Lab For Implementing Branch Instructions
    * RV_D4SK3_L7_Lab For Completing Branch Instruction Implementation
    * RV_D4SK3_L8_Lab To Create Simple Testbench
* [5. RV Day 5 - Complete Pipelined RISC-V CPU micro-architecture](#5--rv-day-5---complete-pipelined-risc-v-cpu-micro-architecture)
  * RV-D5SK1 - Pipelining the CPU
    * RV_D5SK1_L1_Introduction To Control Flow Hazard And Read After Write Hazard
    * RV_D5SK1_L2_Lab To Create 3-Cycle Valid Signal
    * RV_D5SK1_L3_Lab To Code 3-Cycle RISC-V To Take Care Of Invalid Cycles
    * RV_D5SK1_L4_Lab To Modify 3-Cycle RISC-V To Distribute Logic 
  * RV-D5SK2 - Solutions to Pipeline Hazards
    * RV-D5SK2 - Solutions to Pipeline Hazards
    * RV_D5SK2_L1_Lab For Register File Bypass To Address Rd-After-Wr Hazard
    * RV_D5SK2_L2_Lab For Branches To Correct The Branch Target Path
    * RV_D5SK2_L3_Lab To Complete Instruction Decode Except Fence, Ecall, Ebreak
    * RV_D5SK2_L4_Lab To Code Complete ALU
  * RV-D5SK3 - Load/Store Instructions and Completing RISC-V CPU
    * RV_D5SK3_L1_Introduction To Load Store Instructions And Lab To Redirect Loads
    * RV_D5SK3_L2_Lab To Load Data From Memory To Register File
    * RV_D5SK3_L3_Lab To Instantiate Data Memory To The CPU
    * RV_D5SK3_L4_Lab To Add Stores And Loads To The Test Program
    * RV_D5SK3_L5_Lab To Add Control Logic For Jump Instructions
    * RV_D5SK3_L6_Wrap Up

