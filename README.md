# RISC-V
## Table of Contents  
* [1. RV DAY 1 Introduction to RISC-V ISA and GNU compiler toolchain](#1--rv-day-1-introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
  * [RV-D1SK1 - Introduction to RISC-V basic keywords](#rv-d1sk1---introduction-to-risc-v-basic-keywords)
  * [RV-D1SK2 - Labwork for RISC-V software toolchain](#rv-d1sk2---labwork-for-risc-v-software-toolchain)
  * [RV-D1SK3 - Integer number representation](#rv-d1sk3---integer-number-representation)
* [2. RV Day 2 - Introduction to ABI and basic verification flow](#2--rv-day-2-introduction-to-abi-and-basic-verification-flow)
  * [RV-D2SK1 - Application Binary interface (ABI)](#rv-d2sk1---application-binary-interface-(abi))
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

### <a name="rv-d1sk2---labwork-for-risc-v-software-toolchain"></a> RV-D1SK2 - Labwork for RISC-V software toolchain ###

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
Some data representation terms we use:   
_Byte_ - A byte is a fundamental unit of digital information that consists of a group of eight bits.   
_Word_ - A word is a basic unit of data that a processor can operate on in a single instruction. It typically corresponds to the natural data width of the processor's architecture. In RISC-V Word is of length 32bits.  
_Double Word_ - In computer architecture and data representation, a "double word" is a term used to describe a unit of data that is twice the size of a "word." In RISC-V Double word is of length 64bits.  
_Most Significant bit(MSB)_ - MSB stands for "Most Significant Bit." It is a term used in digital systems and binary representation to refer to the bit in a binary number that holds the highest positional value.   
_Least Significant bit(LSB)_ - LSB stands for "Least Significant Bit." It is the term used in binary representation to refer to the bit in a binary number that holds the lowest positional value. In other words, the Least Significant Bit is the rightmost bit in a binary representation.  

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/c30bdb1a-b5e9-457e-b48c-ecc4661aca33)

The total number of represented by RISC-V64 is:
![Screenshot from 2023-08-19 22-12-13](https://github.com/V-Pranathi/RISC-V/assets/140998763/8e128a4d-3495-4eb3-b80b-efedb951edd5)

The maximum and minimum unsigned value it holds is:
![Screenshot from 2023-08-19 22-12-55](https://github.com/V-Pranathi/RISC-V/assets/140998763/120f561d-dcdc-4064-b207-3a805ee988bf)

* When we add two numbers and the sum exceeds the maximum value that it can take, what we should do? For that we have the overflow flag which we will study later.  
* What if I subtract two numbers and resultant is negative? How can we deal with unsigned numbers? Usage of 2,s complement solves the problem.  

**2's complement** The two's complement is a mathematical technique used in computing to represent signed integers (positive and negative whole numbers) using the binary number system.

To convert a negative integer to its two's complement representation:  

1. Take the positive binary representation.  
2. Flip all the bits (change 0s to 1s and 1s to 0s).  
3. Add 1 to the resulting value.   
_Positive number MSB=0_
_Negative number MSB=1_

The maximum and minimum signed value it can hold is:
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/bfa15254-17a8-4bd0-bad0-3e67aae7639e)

Instructions that operate on these kind of numbers(in the below figure) they are called as **Base Integer Instructions  RV64I**
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/a23a7f3f-8ed2-43b0-a928-b9d2cea84619)

C program for Unsigned_highest number in RISC-V 64bit:

	#include <stdio.h>
	#include <math.h>
	int main() {
	unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
	printf("highest number represented by unsigned long long int is %llu\n", max);
	return 0;
 	}

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/38563554-7f14-4453-bec1-244805715555)

Modifying the above program to check whether the result we got is the highest number are not:

 	#include <stdio.h>
	#include <math.h>
	int main() {
	unsigned long long int max = (unsigned long long int) (pow(2,100) -1); 
 	unsigned long long int max = (unsigned long long int) (pow(2,10) -1); 
 	unsigned long long int max = (unsigned long long int) (pow(2,10) * -1); 
	printf("highest number represented by unsigned long long int is %llu\n", max);
	return 0;
 	}
  	//%llu is the format specifier for unsigned integer

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/4dc1ece2-0d39-4bdf-a57b-738fbedb0de2)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/bda93760-3f7f-4852-becc-22ff48bd71ac)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/523e764f-112a-45ba-8e9a-ead116162029)

For double word the lowest unsigned number is zero.    
To get the negative number   

  	#include <stdio.h>
	#include <math.h>
	int main() {
	long long int max = (int) (pow(2,63) -1);
	long long int min = (int) (pow(2,63) * -1);
	printf("highest number represented by long long int is %lld\n", max);
	printf("lowest number represented by long long int is %lld\n", min);
	return 0;
	 }
 	// %lld is the format specifier for signed integer

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/3eb52f7b-2959-4b30-b8ea-00cb861127d2)

We do get the output for negative numbers but it is not the right one.  

The  bug over here is instead of 'int' we have to change it into "long long int"

 	#include <stdio.h>
	#include <math.h>
	int main() {
	long long int max = (long long int) (pow(2,63) -1);
	long long int min = (long long int) (pow(2,63) * -1);
	printf("highest number represented by long long int is %lld\n", max);
	printf("lowest number represented by long long int is %lld\n", min);
	return 0;
 	}
 
 ![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/eb81d9e6-5ad6-448b-aeb3-9e3301433f7e)

## <a name="2--rv-day-2-introduction-to-abi-and-basic-verification-flow"></a> 2. RV Day 2 - Introduction to ABI and basic verification flow ##
### <a name="rv-d2sk1---application-binary-interface-(abi)"></a> RV-D2SK1 - Application Binary interface (ABI)  ###

1. **Application Binary Interface** - In the context of RISC-V, the ABI specifies how system calls are invoked and how data is passed between user-mode applications and the operating system. Specifically, the ABI defines which registers are used for passing parameters to system calls and for receiving return values. It also specifies how the system call number (which corresponds to the specific service being requested) is passed to the operating system.
2. **Register Width in RISC-V** - The width of registers in the RISC-V architecture is determined by the value of XLEN, which represents the native word size of the processor. XLEN is defined at the time the RISC-V architecture is implemented and can be different for different variations of the architecture.
  * For RV64 (RISC-V 64-bit architecture), XLEN is 64 bits. This means that general-purpose registers and other data paths in the processor are 64 bits wide.
  * For RV32 (RISC-V 32-bit architecture), XLEN is 32 bits. In this case, general-purpose registers and data paths are 32 bits wide.
3. **Registers** - In RISC-V there are 32 registers(x1-x31). We can load the registers in two different ways.
  *  Load directly into the registers.
  *  Load into the memory and then to register.(Each memory cell hold 1byte for 64bits data to load into memory we need 8 such memory cells)
4. **Little-endian memory addressing system** - RISC-V belongs to the little endian memory addressing system.In a little-endian system, the least significant byte (LSB) of a multi-byte value is stored at the lowest memory address, while the most significant byte (MSB) is stored at the highest memory address.
     
**LOAD** -"load" instructions are used to fetch data from memory and load it into registers. 
	
    LD (Load Doubleword):
    
    Syntax: LD rd, imm(rs1)
    Description: Loads a 64-bit doubleword from memory at the effective address (rs1 + imm) and stores it in register rd.
    Example: LD x9, 16(x6) loads a 64-bit doubleword from memory at the address stored in x6 plus an offset of 16 and   stores it in x9.
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/9c6f5930-2129-4420-bbb7-f3fe7b26500a)

**ADD** -  "ADD" instruction is used to add the contents of two registers and store the result in a destination register. 
	
    ADD (Add):
  
    Syntax: ADD rd, rs1, rs2
    Description: Adds the value in register rs1 to the value in register rs2 and stores the result in register rd.
    Example: ADD x12, x13, x14 adds the content of register x13 to the content of register x14 and stores the result in x12.
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/66bc0bcd-5c67-47d9-8c2c-39c52014ba5a)

**STORE** - "STORE" instructionis used to store a double-word (64-bit) value from a register into memory.

    SD (Store Doubleword):

    Syntax: SD rs2, imm(rs1)
    Description: Stores a 64-bit doubleword from register rs2 into memory at the effective address (rs1 + imm).
    Example: SD x15, 24(x16) stores the 64-bit value from register x15 into memory at the address stored in x16 plus an offset of 24.
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/d31f693b-26dc-4983-bb84-bff75182e803)

In the RISC-V instruction set architecture (ISA), instructions are categorized based on their formats and functionalities. The I-type, R-type, and S-type instructions are three common categories of instructions in RISC-V. These categories help describe the structure of the instruction and how they operate on data.
  1. I-Type Instructions (Immediate):
        I-type instructions are used for operations that involve an immediate value (constant) and a register.
        The immediate value is encoded within the instruction itself.
        Common examples include ADDI (add immediate), LW (load word), and SW (store word).
        Syntax: OP rd, rs1, imm

  2. R-Type Instructions (Register):
        R-type instructions are used for operations that involve registers.
        The operation is specified by the opcode, and both source registers and destination registers are used.
        Common examples include ADD (add), SUB (subtract), and AND (bitwise AND).
        Syntax: OP rd, rs1, rs2

  3. S-Type Instructions (Store):
	S-type instructions are used for storing data from a register into memory.
	They involve two registers and an offset that determines the memory location.
    	Common examples include SW (store word) and SH (store halfword).
	Syntax: OP rs2, imm(rs1)
_Registers_ As we can see in the above figure 5bits are needed to represent each register. So if we calculate total number of registers we can have it will be, 2^5=32. Different types of registers is shown below.
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/f217f1ac-2e8e-4e06-957a-9389178f777c)
 






