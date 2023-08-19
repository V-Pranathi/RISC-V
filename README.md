# RISC-V
## Table of Contents  
* [1. RV DAY 1 Introduction to RISC-V ISA and GNU compiler toolchain](#1--rv-day-1-introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
  * [RV-D1SK1 - Introduction to RISC-V basic keywords](#rv-d1sk1---introduction-to-risc-v-basic-keywords)
  * RV-D1SK2 - Labwork for RISC-V software toolchain
  * RV-D1SK3 - Integer number representation
* [2. RV Day 2 - Introduction to ABI and basic verification flow](#2--rv-day-2-introduction-to-abi-and-basic-verification-flow)
  * RV-D2SK1 - Application Binary interface (ABI)
  * RV-D2SK2 - Lab work using ABI function calls
  * RV-D2SK3 - Basic verification flow using iverilog
* [3. RV Day 3 - Digital Logic with TL-Verilog and Makerchip](#3--rv-day-3---digital-logic-with-tl-verilog-and-makerchip)
  * RV-D3SK1 - Combinational logic in TL-Verilog using Makerchip
  * RV-D3SK2 - Sequential logic
  * RV-D3SK3 - Pipelined logic
  * RV-D3SK4 - Validity
  * RV Day 3 Wrap-up
* [4. RV Day 4 - Basic RISC-V CPU micro-architecture](#4-rv-day-4---basic-risc-v-cpumicro-architecture)
  * RV-D4SK1 - Introduction to Simple RISC-V Micro-architecture
  * RV-D4SK2 - Fetch and decode
  * RV-D4SK3 - RISC-V control logic
* [5. RV Day 5 - Complete Pipelined RISC-V CPU micro-architecture](#5--rv-day-5---complete-pipelined-risc-v-cpu-micro-architecture)
  * RV-D5SK1 - Pipelining the CPU
  * RV-D5SK2 - Solutions to Pipeline Hazards
  * RV-D5SK3 - Load/Store Instructions and Completing RISC-V CPU
## <a name="1--rv-day-1-introduction-to-risc-v-isa-and-gnu-compiler-toolchain"></a> 1. RV DAY 1 Introduction to RISC-V ISA and GNU compiler toolchain ##  
### <a name="rv-d1sk1---introduction-to-risc-v-basic-keywords"></a> RV-D1SK1 - Introduction to RISC-V basic keywords ###
**RISC-V** - RISC-V is an open-source instruction set architecture (ISA) that is designed to be simple, modular, and extensible. It stands for "Reduced Instruction Set Computing - Five" and is intended to be used for a wide range of applications, from embedded systems to high-performance computing. Unlike many other ISAs, RISC-V is not owned by any single company or organization, which has contributed to its popularity and adoption.  

**ISA** - In the context of computer architecture, "ISA" stands for "Instruction Set Architecture." It refers to the set of instructions that a computer processor can execute, along with the associated encoding, addressing modes, registers, memory organization, and other architectural features. INstructions as  abstract interface between hugh level language and hardware. This abstract interface is nothing but ISA/ Architecture of computer.  

_From apps to Hardware_  

Application softwares/Apps =====> OS(Operating System) =====> Compiler =====>Assembler =====> Hardware  

Particular program runs on a particular layout the flow in betwen the two is the program is compiled in to RISC-V(here) assembly language program and then converted into machine language which is nothing but binary langauge program which gets understood by the hardware of the computer. In this we are mostly focussing on instruction set architecture of RISC-V.  
Consider an example:

    #include <stdio.h>
    int main()
    {
    int num1,num2;
    register int sum;
    num1=45;
    num2=18;
    sum= num1+num2;
    printf("sum of two numbers is:%d", sum);
    return 0;
    }
Output of the compiler looks like

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/d2cd2244-98b1-431c-bb42-efd935b24d41)

In the above image we can see various instructions. In this workshop we will focus on these instructions.
* Pseudo Instructions -- mv,li,ret,.....
* Base integer Instructions RV64I - operates on integers of 64bits. -- lui,addi,sd,auipe,lw,addw,......
* Mutliply extension RV64M -- mulw,divw
* Single and Double precision floating point extenstion RV64F & RV64D -- flw,fadd.s,....
* Application Binray Interface(ABI) -- for keywords like a0,sp,ra,.. programmers  access these registers caues of this interface.
* Memory allocation and stack pointer -- data transfer happening concept called memory allocation  and stack pointer.

### <a name="RV-D1SK2 - Labwork for RISC-V software toolchain"></a> RV-D1SK2 - Labwork for RISC-V software toolchain ###

**GCC compile And Disassemble**

Compiling a C program --> Sum_1ton.c 

    #include<stdio.h>
     int main(){
	    int i,sum=0,n=5;
	    for(i=0;i<=n;i++){
	    sum +=i;
	    }
     printf("sum from 1 to %d is: %d\n",n,sum);
     return 0;
     }

The commands used to compile are given below:

     // gcc compile
     $ riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c
     // disassemble
     $ riscv64-unknown-elf-objdump -d sum.o | less

The output of the compiler is:  

     // helps to see the main file 
     /main
     n

![Screenshot from 2023-08-19 19-48-23](https://github.com/V-Pranathi/RISC-V/assets/140998763/d51200fc-9c88-4242-84e7-30304fcbffbe)
The number of instructions used are: 101b0-10184= 2C ==> 2C/4 = B which means 11 instructions.   

Let us run the same code with slight change in the commands:

     // gcc compile
     $ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum.o sum.c
     // disassemble
     $ riscv64-unknown-elf-objdump -d sum.o | less

The output of the compiler is:

![Screenshot from 2023-08-19 20-04-35](https://github.com/V-Pranathi/RISC-V/assets/140998763/87cfee5a-a5bd-47ab-9489-07d761a42b6c)

**Spike Simulation and debug**
Simulation using gcc

      gcc sum.c
      ./a/out
      
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/bfd66461-9b7b-470b-950a-3cee8d46de90)

Just as above we need to get the output using RISC-V compiler and the command is as follows:

     spike -d pk sum.o

![Screenshot from 2023-08-19 20-12-30](https://github.com/V-Pranathi/RISC-V/assets/140998763/53e87409-20b9-4dc1-b5fe-3b93ef646f81)

Debugging using spike:

	until pc 0 100b0 // runs until oc counter is 100b0
 	reg 0 a2 // contents of a2
  
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/f2d07be9-0b4b-470c-9af0-964817e7cb13)

### <a name="rv-d1sk3---integer-number-representation"></a> RV-D1SK3 - Integer number representation ###











  
