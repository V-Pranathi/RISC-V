# RISC-V
## Table of Contents  
* [1. RV DAY 1 Introduction to RISC-V ISA and GNU compiler toolchain](#1--rv-day-1-introduction-to-risc-v-isa-and-gnu-compiler-toolchain)
  * [RV-D1SK1 - Introduction to RISC-V basic keywords](#rv-d1sk1---introduction-to-risc-v-basic-keywords)
  * [RV-D1SK2 - Labwork for RISC-V software toolchain](#rv-d1sk2---labwork-for-risc-v-software-toolchain)
  * [RV-D1SK3 - Integer number representation](#rv-d1sk3---integer-number-representation)
* [2. RV Day 2 - Introduction to ABI and basic verification flow](#2--rv-day-2-introduction-to-abi-and-basic-verification-flow)
  * [RV-D2SK1 - Application Binary interface (ABI)](#rv-d2sk1---application-binary-interface-(abi))
  * [RV-D2SK2 - Lab work using ABI function calls](#rv-d2sk2---lab-work-using-abi-function-calls)
  * [RV-D2SK3 - Basic verification flow using iverilog](#rv-d2sk3---basic-verification-flow-using-iverilog)
* [3. RV Day 3 - Digital Logic with TL-Verilog and Makerchip](#3--rv-day-3---digital-logic-with-tl-verilog-and-makerchip)
  * [RV-D3SK1 - Combinational logic in TL-Verilog using Makerchip](#rv-d3sk1---combinational-logic-in-tl-verilog-using-makerchip)
  * [RV-D3SK2 - Sequential logic](#rv-d3sk2---sequential-logic)
  * [RV-D3SK3 - Pipelined logic](#rv-d3sk3---pipelined-logic)
  * [RV-D3SK4 - Validity](#rv-d3sk4---validity)
* [4. RV Day 4 - Basic RISC-V CPU micro-architecture](#4-rv-day-4---basic-risc-v-cpumicro-architecture)
  * [RV-D4SK1 - Introduction to Simple RISC-V Micro-architecture](#rv-d4sk1---introduction-to-simple-risc-v-micro-architecture)
  * [RV-D4SK2 - Fetch and decode](#rv-d4sk2---fetch-and-decode)
  * [RV-D4SK3 - RISC-V control logic](#rv-d4sk3---risc-v-control-logic)
* [5. RV Day 5 - Complete Pipelined RISC-V CPU micro-architecture](#5--rv-day-5---complete-pipelined-risc-v-cpu-micro-architecture)
  * [RV-D5SK1 - Hazards](#rv-d5sk1---hazards)
  * [RV-D5SK2 - RISC-V CPU Pipelined core](#rv-d5sk2---risc-v-cpu-pipelined-core)
* [7. Contributors](#6--Contributors)
* [6. References](#6--references)
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

### <a name="rv-d2sk2---lab-work-using-abi-function-calls"></a> RV-D2SK2 - Lab work using ABI function calls ###

Using ASM(Assembly language program) we are writing the code for sum of numbers from 1 to 9. We have main program in 1to9_custom.c file which does the function call and the function get calls in load.S

	//custom_1to9.c
	#include<stdio.h>
	extern int load(int x,int y); //defined function over here

	int main{
	int result=0;
  	int count=9;
	result = load(0x0, count+1);

	printf("sum of numbers from 1 to %d is %d\n", count,result);
	}


	//load.S
 	.section .text	
	.global load
	.type load, @function

	load: 
		add  a4, a0, 0
		add  a2, a1, 0
		add  a3, 0, 0
	loop:
		add  a4, a4, a3
		add  a3, a3, 1
		blt  a3, a2, loop
		add  a0, a4, 0
		ret
_Output of sum of numbers from 1 to 9 using ASM function calling from C program_:

	$ riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o 1to9_custom.o 1to9_custom.c load.S
	~$ spike pk 1to9_custom.o

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/2afb3ceb-05c1-4e01-bd05-94bf7438e1c3)


_Object dump_:
	$ riscv64-unknown-elf-objdump -d 1to9_custom.o | less

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/bb82d71c-78b4-457c-8a45-9e5bacf80ed3)

**Lab to run C-program on RISC-V CPU** The flow is shown below
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/af19b2a3-3d8d-4d8f-aa91-95b27b8976b4)

### <a name="rv-d2sk3---basic-verification-flow-using-iverilog"></a> RV-D2SK3 - Basic verification flow using iverilog ###
There is this rv32im.sh file which is the script file which contain scripts that are needed to convert into hex file which is firmware.hex and load it into picor32.v memory using testbench.v and simulate it at the end.

rv32im.sh file contains

 	riscv64-unknown-elf-gcc -c -mabi=ilp32 -march=rv32im -o 1to9_custom.o 1to9_custom.c 
	riscv64-unknown-elf-gcc -c -mabi=ilp32 -march=rv32im -o load.o load.S

	riscv64-unknown-elf-gcc -c -mabi=ilp32 -march=rv32im -o syscalls.o syscalls.c
	riscv64-unknown-elf-gcc -mabi=ilp32 -march=rv32im -Wl,--gc-sections -o firmware.elf load.o 1to9_custom.o syscalls.o -T riscv.ld -lstdc++
	chmod -x firmware.elf
	riscv64-unknown-elf-gcc -mabi=ilp32 -march=rv32im -nostdlib -o start.elf start.S -T start.ld -lstdc++
	chmod -x start.elf
	riscv64-unknown-elf-objcopy -O verilog start.elf start.tmp
	riscv64-unknown-elf-objcopy -O verilog firmware.elf firmware.tmp
 	 cat start.tmp firmware.tmp > firmware.hex
	python3 hex8tohex32.py firmware.hex > firmware32.hex
	rm -f start.tmp firmware.tmp
	iverilog -o testbench.vvp testbench.v picorv32.v
  	chmod -x testbench.vvp
	vvp -N testbench.vvp

Inorder to run rv32im.sh file steps involved are:

	$ chmod 777 rv32im.sh // change the permissions
	$ ./rv32im.sh 

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/36900341-5408-4d55-bdf6-f71230d85dfb)

## <a name="3--rv-day-3---digital-logic-with-tl-verilog-and-makerchip3"></a> RV Day 3 - Digital Logic with TL-Verilog and Makerchip ##
### <a name="rv-d3sk1---combinational-logic-in-tl-verilog-using-makerchip"></a> RV-D3SK1 - Combinational logic in TL-Verilog using Makerchip ###
**Introduction to Logic gates**  
Logic gates are fundamental building blocks of digital circuits and are used to perform logical operations on binary data, which consists of 0s and 1s. These gates are the foundational components that allow computers and other digital devices to process and manipulate information.  
There are several types of basic logic gates, each performing a specific logical operation:  
![Screenshot from 2023-08-20 17-40-35](https://github.com/V-Pranathi/RISC-V/assets/140998763/ce5959f9-92bc-427a-981c-b6eaaa13bf79)

**TL-verilog** which is Transactional - Level verilog is a higher-level abstraction of hardware description language (HDL) used for modeling and designing digital systems at a higher level of abstraction. It's often used to describe the behavior of a system without delving into the low-level implementation details. This level of abstraction is particularly useful for system-level design and simulation.  
In contrast to the traditional gate-level Verilog, which focuses on describing the circuit interconnections and physical gates, transactional-level Verilog allows designers to describe the operation of the system using more abstract constructs.  

**Makerchip** Makerchip is an online platform and integrated development environment (IDE) that allows users to design, simulate, and implement digital systems using hardware description languages (HDLs) like TL-verilog, Verilog and VHDL. It provides a user-friendly environment for creating and testing digital designs, making it especially useful for learning, teaching, and prototyping digital circuits and systems.  
This is how makerchip platform looks like(below is the example of pythagorean)
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/2542e664-d829-4fcc-a6d5-a04478cdc929)

**(A) Inverter on makerchip** 

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/31d2a237-099a-4c7b-bce1-88144fde705f)

**(B) OR gate on makerchip**

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/d2582d1d-99a9-4a01-934c-171ebbe4d1d1)

**(C) Vectors on makerchip**

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/82787089-1f99-4c1b-a5a0-a488bcdda3d7)

**(D) 1 bit Multiplexer on makerchip**

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/8dc0b851-29b1-4cc3-9014-a0e4647d3c3d)

**(E) Multiplexer on makerchip**

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/c096e8ee-de40-4fdc-82d9-4cfbd212e91e)

**(F) Calculator on makerchip**

	\TLV
  	 $reset = *reset;
   
  	 $val1[31:0] = $rand1[3:0];
  	 $val2[31:0] = $rand2[3:0];
   
  	 $sum[31:0] = $val1 + $val2;
  	 $diff[31:0] = $val1 - $val2;
  	 $prod[31:0] = $val1 * $val2;
  	 $quot[31:0] = $val1 / $val2;
   
   	$out[31:0] = $opt[1] ? ($opt[0] ? $quot : $prod) : ($opt[0] ? $diff : $sum);
    
   	*passed = *cyc_cnt > 40;
   	*failed = 1'b0;
	\SV
 	  endmodule
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/79c59025-ef99-4f5d-9f20-fd64e090f7ee)

### <a name="rv-d3sk2---sequential-logic"></a> RV-D3SK2 - Sequential logic ###

**(A) Fibonacci sequence on makerchip**

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/4775a2d8-057e-48de-b962-d87823077b21)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/60d65a7a-2222-4526-9dea-dbaf4446ff47)


**(B) Counter on makerchip**

	\TLV
  	 $reset = *reset;
   
 	 $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1'b1);
   
  	 *passed = *cyc_cnt > 40;
  	 *failed = 1'b0;
	 \SV
 	 endmodule
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/8c3f2a86-b61e-4555-912e-d282de928751)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/14c708fd-bf30-43e8-91c3-44e2dad318f3)

**(C) Sequential calculator on makerchip**

         \TLV
	 $reset = *reset;
   
  	 $val2[31:0] = $rand2[3:0];
  	 $val1[31:0] = >>1$out[31:0];
   
  	 $sum[31:0] = $val1 +  $val2;
  	 $diff[31:0] = $val1 - $val2;
   	 $prod[31:0] = $val1 * $val2;
  	 $quot[31:0] = $val1 / $val2;
   
  	 $out[31:0] = $reset ? 0 : ($opt[1] ? ($opt[0] ? $quot : $prod) : ($opt[0] ? $diff : $sum));
    
   	*passed = *cyc_cnt > 40;
   	*failed = 1'b0;
	\SV
   	endmodule
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/c70f5e27-285f-422f-b8df-f95e96634c78)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/ac161209-5cfe-4c7f-ac24-095190dfd683)

### <a name="rv-d3sk3---pipelined-logic"></a> RV-D3SK3 - Pipelined logic ###

**Pipeline - Timing abstract**  Pipeline timing abstraction in Transaction-Level (TL) Verilog involves modeling the behavior of a pipelined digital system at a higher level of abstraction. Pipelining is a technique used to improve the throughput of digital systems by breaking down a complex operation into multiple stages, allowing multiple operations to be in progress simultaneously.  Pipeline timing helps to operate at higher frequency.

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/b1908ce8-0b9c-4688-ae0a-ef6e7a10c7f1)

**Identifiers and types in TL verilog**  In Transaction-Level (TL) Verilog, identifiers are names used to represent various elements in your design, such as modules, signals, variables, and instances. Identifiers follow certain naming conventions and rules to ensure readability, consistency, and compatibility with the Verilog language.  

_Naming Conventions:_
      1.  $lower_case: Used for pipe signals, which represent communication between modules or stages in a pipeline.
      2.  $CamelCase: Used for state signals, which often represent the state of a finite state machine (FSM).
      3.  $Upper_CASE: Used for keyword signals, which typically represent control signals or special flags.  
      
_Identifier Structure:_
      1. The first token of an identifier must start with at least two alphabetical characters (letters).
      2. Identifiers can include numbers at the end of tokens, but they should follow the naming rules you've outlined. For example, $base64_value is valid, but $base_64 is not. 

**Errors within computation pipeline**  

	\TLV
   	$reset = *reset;
   
   	|comp
      @1
         $err1 = $bad_input || $illegeal_op;
      @3
         $err2 = $err1 || $over_flow;
      @6
         $err3 = $err2 || $div_by_zer0;

   	*passed = *cyc_cnt > 40;
  	 *failed = 1'b0;
	\SV
  	 endmodule
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/6e533edf-2f41-447e-bd11-dbd7acd48709)

**Calculator and Counter in Pipeline**  
The block diagram:
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/ca020732-9fbe-4391-8b6f-353bddaa9e3d)

	\TLV
  	 $reset = *reset;
   
   	$val2[31:0] = $rand2[3:0];
   
  	 |calc
     	 @1
         
         $val1[31:0] = >>1$out[31:0];
   
         $sum[31:0] = $val1 +  $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;
   
         $out[31:0] = $reset ? 32'b0 : ($opt[1] ? ($opt[0] ? $quot : $prod) : ($opt[0] ? $diff : $sum));
         $cnt[31:0] = $reset ? 0 : (>>1$cnt + 1'b1);
  
   	*passed = *cyc_cnt > 40;
   	*failed = 1'b0;
	\SV
   	endmodule
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/c720fa7d-3105-4198-b5c4-bfe8cba7e625)

**(B) 2-Cycle Calculator**  
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/ba1938b8-3c35-45f3-b30e-5b91b8afa0fd)

  	\TLV
  	 $reset = *reset;
   
   	 $val2[31:0] = $rand2[3:0];
   
  	 |calc
      	 @1
         
         $val1[31:0] = >>1$out[31:0];
   
         $sum[31:0] = $val1 +  $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;
         $valid = $reset ? 0 : (>>1$valid + 1'b1);
      
      @2
         
         $out[31:0] = ($reset || !($valid)) ? 32'b0 : ($opt[1] ? ($opt[0] ? $quot : $prod) : ($opt[0] ? $diff : $sum));
        
   	*passed = *cyc_cnt > 40;
  	 *failed = 1'b0;
	\SV
  	 endmodule

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/468ca649-d649-47ba-b802-5f49b7d3d446)

### <a name="rv-d3sk4---validity"></a> RV-D3SK4 - Validity ###

**Validity:** In Transaction-Level (TL) Verilog, validity refers to the state of a signal that indicates whether a particular piece of data or information is valid or not. It's commonly used in designs involving communication between different modules or stages, such as in pipelined architectures or when dealing with data transfers between components. Validity signals help control the flow of data and ensure that the data being processed is meaningful and accurate.  

Validity signaling is especially important when working at a higher level of abstraction, as it allows you to model data transfers and interactions without diving into the low-level details of timing and specific data transitions.  

Validity provides:
* Easier debug
* Cleaner design
* Better error checking
* Automated clock gating

**Clock Gating:** Clock gating is a power-saving technique used in digital circuit design to reduce the dynamic power consumption of a circuit by controlling the clock signal to specific components or modules when they are not actively needed. TL-Verilog can produce fine-grained gating(Enable). It involves enabling or disabling the clock signal to certain portions of a circuit based on their operational requirements. This helps conserve power by reducing unnecessary clock switching and activity. In clock gating, the clock signal is used as a control signal to enable or disable the operation of certain elements within a design. When a component's clock is gated off (disabled), it effectively stops processing and consuming power, thereby reducing power consumption. When the component's clock is gated on (enabled), it resumes normal operation.  

**(A) Distance Accumulator on makerchip**  

	\TLV
  	 $reset = *reset;

  	 |calc
     	 @1
         $reset = *reset;
            
      ?$vaild      
         @1
            $aa_seq[31:0] = $aa[3:0] * $aa;
            $bb_seq[31:0] = $bb[3:0] * $bb;;
      
         @2
            $cc_seq[31:0] = $aa_seq + $bb_seq;;
      
         @3
            $cc[31:0] = sqrt($cc_seq);
            
      @4
         $total_distance[63:0] = 
            $reset ? '0 :
            $valid ? >>1$total_distance + $cc :
                     >>1$total_distance;
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/fb4c9612-b12a-4682-bb77-0fe479231162)

**(B) Cycle Calculator with Validity**
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/fef7517b-1b13-4b96-b044-9aa8bb6fe335)

	|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out [31:0];
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $out [31:0] = $reset ? 32'b0 :
                          ($op[1:0] == 2'b00) ? $sum :
                          ($op[1:0] == 2'b01) ? $diff :
                          ($op[1:0] == 2'b10) ? $prod :
                                                $quot ;
            
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/e5c6d5a5-1aba-46b8-9b8d-0ec850f0d4d3)

**(C) Calculator with single value memory**
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/66e95146-0fea-4002-a599-d385cdd4e9ac)

	|calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out;
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $quot[31:0] = $val1 / $val2;
            
         @2   
            $mem[31:0] = $reset ? 32'b0 :
                         ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
            
            $out [31:0] = $reset ? 32'b0 :
                          ($op[2:0] == 3'b000) ? $sum :
                          ($op[2:0] == 3'b001) ? $diff :
                          ($op[2:0] == 3'b010) ? $prod :
                          ($op[2:0] == 3'b011) ? $quot :
                          ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/899e8e76-21ed-433c-b027-30d5015aa110)

## <a name="4-rv-day-4---basic-risc-v-cpumicro-architecture"></a> 4. RV Day 4 - Basic RISC-V CPU micro-architecture ##
### <a name="rv-d4sk1---introduction-to-simple-risc-v-micro-architecture"></a> RV-D4SK1 - Introduction to Simple RISC-V Micro-architecture ###
**RISC-V block diagram**  A block diagram of a RISC-V processor provides a high-level overview of its major components and how they are interconnected. Here's a block diagram of a typical RISC-V processor:
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/cfe37ccc-c7e9-4cce-ae86-6e1ae69831f5)

**1. Decoder:** The decoder is a crucial component in a processor that takes an instruction as input (typically in binary form) and decodes it to generate control signals that direct the various functional units within the processor. It determines the operation to be performed, the registers involved, memory access requirements, and other microoperations associated with the instruction. The decoder's output controls the operation of subsequent stages in the pipeline.

**2. Instruction Memory:** The instruction memory (often referred to as the "instruction cache") is responsible for storing the machine instructions that the processor fetches and decodes. Instructions are read from memory addresses specified by the program counter (PC). The instruction memory supplies the fetched instruction to the decoder, which then extracts and decodes the opcode and operands.

**3. ALU (Arithmetic Logic Unit):** The ALU is a functional unit within the processor responsible for performing arithmetic and logical operations on data. It can perform tasks like addition, subtraction, bitwise operations, comparisons, shifts, and more. The ALU takes input operands from registers and produces results that might be stored back in registers or used for subsequent operations.

**4. ALU Control Unit:** The ALU control unit generates control signals to configure the ALU for the specific operation required by the instruction being executed. For example, it might determine whether the ALU should perform addition, subtraction, bitwise AND, OR, etc., based on the instruction's opcode and any associated immediate values.

**5. Register File:** The register file is a storage component that holds the processor's general-purpose registers. These registers are used for temporary storage of data during instruction execution. In RISC-V, the register file contains a fixed number of registers (e.g., 32 registers in a standard RISC-V implementation).

**6. Data Memory:** The data memory (often referred to as the "data cache") is used for reading from and writing to data values in memory. It holds the data used in memory load and store operations. Data memory access can be more time-consuming than register access due to the latency associated with memory hierarchy, caches, and main memory.  

### <a name="rv-d4sk2---fetch-and-decode"></a> RV-D4SK2 - Fetch and decode ###

**(A) PC increment**  

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/16449a61-d56a-4c99-ab63-7653a3e38bfd)

	|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 0 : (>>1$pc + 32'd4);

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/e65a78a8-1554-4597-8793-b53ac2301d5e)

**(B) Fetch** 

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/b1e7329a-a157-4bf4-ade2-32303c799b72)

	|cpu
      @0
         $reset = *reset;
         $pc[31:0] = >>1$reset ? 0 : (>>1$pc + 32'd4);
      
      @1 
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = imem[$imem_rd_addr]$instr;
	    
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/ceef5678-8c4c-44c0-84ad-040887eb8d5a)

**(C)Instruction Types Decode Logic**  
In the Instruction Decode logic, all the instructions are decoded for the type of instruction, immediate instructions and the field type instructions. The opcode values are translated into instructions, and all the bit values are interpreted as per defined in the RISC-V ISA.

At first, the Instruction type is decoded using 5 bits of the instruction instr[6:2]. The lower two bits from [1:0] are always equal to '11' for Base integer instructions. 

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/230bcef4-09fc-4975-bdaa-a98d09f8cf4c)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/f81254c2-e3d8-426b-b7f3-c3dcb09d12cc)

	   |cpu
      @0
         $reset = *reset;
         
         //NEXT PC
         $pc[31:0] = >>1$reset ? 32'b0 : >>1$pc + 32'd4;
         
         //INSTRUCTION FETCH
      @1
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
            
            
         //INSTRUCTION TYPES DECODE         
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
	 
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/8afd9f38-5fea-44af-a54e-551b002b4fa0)

**(D) Instruction decode logic**

	  //INSTRUCTION IMMEDIATE DECODE
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         
         
         //INSTRUCTION DECODE
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
         
         
         //INSTRUCTION DECODE
         $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/2b1f7099-e374-4e2e-a19a-be91d69f0998)

### <a name="rv-d4sk3---risc-v-control-logic"></a> RV-D4SK3 - RISC-V control logic ###

**(A) Register file read**  

	//REGISTER FILE READ
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
	 
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/d85a62d0-2cac-4a65-98fe-ea7c69da8d86)

**(B) ALU**

	//ARITHMETIC AND LOGIC UNIT (ALU)
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/91984b31-9fdb-4750-b94a-d45edebf4085)

**(C) Register File Write**

	//REGISTER FILE WRITE
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/fff88544-910c-4129-bfd1-e566bc73ef09)

**(D) Branch instructions**  

 	//BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;
	 
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/bc79d3c9-08c9-439e-973a-80a20af21458)

**(E) Test bench**

         //TESTBENCH
          *passed = |cpu/xreg[10]>>5$value == (1+2+3+4+5+6+7+8+9) ;

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/a8ec5e60-8646-456e-89dd-e37b8e7d537d)

## <a name="5--rv-day-5---complete-pipelined-risc-v-cpu-micro-architecture"></a> 5. RV Day 5 - Complete Pipelined RISC-V CPU micro-architecture ##
### <a name="rv-d5sk1---hazards"></a> RV-D5SK1 - Hazards ###  
**Control flow hazards and Read-after-write hazards**  

Control flow hazards and read-after-write hazards are two types of hazards that can occur in computer architectures, including the RISC-V architecture. These hazards can impact the proper execution and performance of programs. Let's delve into each of these hazards:  

_Control Flow Hazard:_ A control flow hazard occurs when the outcome of a conditional branch instruction is not known at the time the subsequent instructions are fetched. This can lead to incorrect instruction execution and disrupt the normal program flow. Control flow hazards are especially prevalent in pipelined architectures where instructions are fetched, decoded, executed, and completed in separate stages.

In the context of the RISC-V architecture, a common example of a control flow hazard is a conditional branch instruction (e.g., beq, bne, etc.). If the branch instruction's outcome depends on the result of a previous instruction that is still in the pipeline, the pipeline might need to be stalled or flushed to ensure correct execution.

_Read-After-Write Hazard (RAW Hazard):_ A read-after-write hazard occurs when an instruction tries to read a data value that is still being written to by a previous instruction. This can lead to incorrect results if not handled properly. In a pipeline, if one instruction writes to a register and a subsequent instruction reads from the same register, and there's no intervening instruction to ensure the write is completed before the read, a hazard occurs.

In RISC-V, like in many other architectures, the hazard can be mitigated using techniques like forwarding (also known as bypassing) or stalling. Forwarding involves forwarding the data directly from the execution stage of the producing instruction to the operand stage of the consuming instruction. Stalling involves introducing "bubble" stages in the pipeline to allow time for the write operation to complete before the read operation is executed.

**(A)Cycle Valid Instructions**  

	@3
         //CYCLE VALID INSTRUCTIONS
         $valid = !(>>1$valid_taken_branch || >>2$valid_taken_branch || 
                    >>1$valid_load || >>2$valid_load ||  
                    >>1$valid_jump || >>2$valid_jump) ;
         
         $valid_load = $valid && $is_load ;
         
         $valid_jump = $is_jump && $valid ;

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/5dc8d7d4-b291-4c2e-9ef4-154dbfbf2fd2)

**(B) ALU**

 	@3   
         //ARITHMETIC AND LOGIC UNIT (ALU)
         
         $sltu_rslt[31:0] = $src1_value < $src2_value ;
         $sltiu_rslt[31:0]  = $src1_value < $imm ;
         
         $result[31:0] = $is_andi ? $src1_value & $imm :
                         $is_ori ? $src1_value | $imm :
                         $is_xori ? $src1_value ^ $imm :
                         ($is_addi || $is_load || $is_s_instr) ? $src1_value + $imm :
                         $is_slli ? $src1_value << $imm[5:0] :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_and ? $src1_value & $src2_value :
                         $is_or ? $src1_value | $src2_value :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_add ? $src1_value + $src2_value :
                         $is_sub ? $src1_value - $src2_value :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_srl ? $src1_value >> $src2_value[4:0] :
                         $is_sltu ? $src1_value | $src2_value :
                         $is_sltiu ? $src1_value < $imm :
                         $is_lui ? {$imm[31:12], 12'b0} :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 :
                         $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                         $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                         $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                         $is_sra ? {{32{$src1_value[31]}}, $src1_value} > $src2_value[4:0] :
                         32'bx ;
![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/aaa1e211-d2a6-4a39-867e-6b23a037c5ae)

**(C) Branch Instructions**  

	 	//BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         
         $valid_taken_branch = $valid && $taken_branch;
                  
      @2
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/97a7585c-d2cb-49bb-b27a-32879f03b6f4)

**(D) Register memory**  

	 @4
         //MINI 1-R/W MEMORY
         $dmem_wr_en = $is_s_instr && $valid ;
         $dmem_addr[3:0] = $result[5:2] ;
         $dmem_wr_data[31:0] = $src2_value ;
         $dmem_rd_en = $is_load ;

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/976b2a40-4526-45eb-95f6-ca82679cac7e)


### <a name="rv-d5sk2---risc-v-cpu-pipelined-core"></a> RV-D5SK2 - RISC-V CPU Pipelined core  ###  

The RISC-V core was developed on Day_4. Now we implement the pipelining technique for the core, and some additonal instructions. The above architecture is pipelined as follows :

* A 3 cycle RISC V pipelined core, with all the base integer instruction sets was developed.
* For Load and store a Data memory element was added with neccessary instruction decoding logic.
* Register Bypass and Squashing techniques were also incorporated to prevent Read followed by write and branching hazards, arised due to pipelining.
* Testing of the pipeline design was done in same manner with Load and store incorporated in asm code.
* Additionally Incorporation of Jump feature (JAL and JALR instructions) was also done.

		\m4_TLV_version 1d: tl-x.org
		\SV
  		//  This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
  		 m4_include_lib(['https://raw.githubusercontent.com/stevehoover/RISC-V_MYTH_Workshop/c1719d5b338896577b79ee76c2f443ca2a76e14f/tlv_lib/risc-v_shell_lib.tlv'])

		\SV
	   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
		\TLV

  		// /====================\
   		// | Sum 1 to 9 Program |
   		// \====================/
   		//
   		// Program for MYTH Workshop to test RV32I
   		// Add 1,2,3,...,9 (in that order).
		//
   		// Regs:
   		//  r10 (a0): In: 0, Out: final sum
  		 //  r12 (a2): 10
   		//  r13 (a3): 1..10
   		//  r14 (a4): Sum
  		 // 
   		// External to function:
  	 	m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   		// Function:
   		m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   		m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   		m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   		// Loop:
  		 m4_asm(ADD, r14, r13, r14)           // Incremental addition
  		 m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
  		 m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
  		 m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
  		 m4_asm(SW, r0, r10, 10000)           // Store the final result value to byte address 16
  		 m4_asm(LW, r17, r0, 10000)           // Load the final result value from adress 16 to x17
   
   		// Optional:
 	 	// m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
  		 m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)
   
  		 |cpu
      @0
         $reset = *reset;
         
         //NEXT PC LOGIC       
         
         $pc[31:0] = >>1$reset ? 32'b0 :
                     >>3$valid_taken_branch ? >>3$br_target_pc :
                     >>3$valid_load ? >>3$inc_pc :
                     >>3$valid_jump && >>3$is_jal ? >>3$br_target_pc :
                     >>3$valid_jump && >>3$is_jalr ? >>3$jalr_target_pc :
                     >>1$inc_pc ;
      @1   
         $inc_pc[31:0] = $pc + 32'd4;
         
         
      @3
         //CYCLE VALID INSTRUCTIONS
         $valid = !(>>1$valid_taken_branch || >>2$valid_taken_branch || 
                    >>1$valid_load || >>2$valid_load ||  
                    >>1$valid_jump || >>2$valid_jump) ;
         
         $valid_load = $valid && $is_load ;
         
         $valid_jump = $is_jump && $valid ;
         
         
         
         //INSTRUCTION FETCH LOGIC
      @1 
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $imem_rd_en = !$reset;
         $instr[31:0] = $imem_rd_data[31:0];
         
         
         //INSTRUCTION TYPES DECODE   
      @1
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         
         //INSTRUCTION IMMEDIATE DECODE
         
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         `BOGUS_USE($imm)
         
         $opcode[6:0] = $instr[6:0];
         
         
         //INSTRUCTION FIELD DECODE
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
            
         `BOGUS_USE($rd)
         
      @2
         //INSTRUCTION DECODE
         $dec_bits[10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
         $is_load = $opcode == 7'b0000011;
         
         $is_xori = $dec_bits ==? 11'bx_100_0010011;
         $is_xor = $dec_bits ==? 11'b0_100_0110011;
         $is_sw = $dec_bits ==? 11'bx_010_0100011;
         $is_sub = $dec_bits ==? 11'b1_000_0110011;
         $is_srli = $dec_bits ==? 11'b0_101_0010011;
         $is_srl = $dec_bits ==? 11'b0_101_0110011;
         $is_srai = $dec_bits ==? 11'b1_101_0010011;
         $is_sra = $dec_bits ==? 11'b1_101_0110011;
         $is_sltu = $dec_bits ==? 11'b0_011_0110011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_slti = $dec_bits ==? 11'bx_010_0010011;
         $is_slt = $dec_bits ==? 11'b0_010_0110011;
         $is_slli = $dec_bits ==? 11'b0_001_0010011;
         $is_sll = $dec_bits ==? 11'b0_001_0110011;
         $is_sh = $dec_bits ==? 11'bx_001_0100011;
         $is_sb = $dec_bits ==? 11'bx_000_0100011;
         $is_ori = $dec_bits ==? 11'bx_110_0010011;
         $is_or = $dec_bits ==? 11'b0_110_0110011;
         $is_lui = $dec_bits ==? 11'bx_xxx_0110111;
         $is_jalr = $dec_bits ==? 11'bx_000_1100111;
         $is_jal = $dec_bits ==? 11'bx_xxx_1101111;
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_andi = $dec_bits ==? 11'bx_111_0010011;
         $is_and = $dec_bits ==? 11'b0_111_0110011;
         
         $jalr_target_pc[31:0] = $src1_value +$imm ;
         
      @3
         $is_jump = $is_jal || $is_jalr ;
         
         `BOGUS_USE ($is_beq $is_bne $is_blt $is_bge $is_bltu $is_bgeu $is_addi $is_add)
         
         
      @2
         //REGISTER FILE READ
         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
      @3
         //REGISTER FILE WRITE
         $rf_wr_en = ($rd_valid && $rd != 5'b0 && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = >>2$valid_load ? >>2$rd : $rd;
         $rf_wr_data[31:0] = >>2$valid_load ? >>2$ld_data : $result;
         
      @2   
         $src1_value[31:0] = (>>1$rf_wr_index == $rf_rd_index1) && >>1$rf_wr_en ? >>1$result :  
                             $rf_rd_data1;
         
         $src2_value[31:0] = (>>1$rf_wr_index == $rf_rd_index2) && >>1$rf_wr_en ? >>1$result :
                             $rf_rd_data2;
         
      @4
         //MINI 1-R/W MEMORY
         $dmem_wr_en = $is_s_instr && $valid ;
         $dmem_addr[3:0] = $result[5:2] ;
         $dmem_wr_data[31:0] = $src2_value ;
         $dmem_rd_en = $is_load ;
         
      @5
         //LOAD DATA
         $ld_data[31:0] = $dmem_rd_data ;
         
         
      @3   
         //ARITHMETIC AND LOGIC UNIT (ALU)
         
         $sltu_rslt[31:0] = $src1_value < $src2_value ;
         $sltiu_rslt[31:0]  = $src1_value < $imm ;
         
         $result[31:0] = $is_andi ? $src1_value & $imm :
                         $is_ori ? $src1_value | $imm :
                         $is_xori ? $src1_value ^ $imm :
                         ($is_addi || $is_load || $is_s_instr) ? $src1_value + $imm :
                         $is_slli ? $src1_value << $imm[5:0] :
                         $is_srli ? $src1_value >> $imm[5:0] :
                         $is_and ? $src1_value & $src2_value :
                         $is_or ? $src1_value | $src2_value :
                         $is_xor ? $src1_value ^ $src2_value :
                         $is_add ? $src1_value + $src2_value :
                         $is_sub ? $src1_value - $src2_value :
                         $is_sll ? $src1_value << $src2_value[4:0] :
                         $is_srl ? $src1_value >> $src2_value[4:0] :
                         $is_sltu ? $src1_value | $src2_value :
                         $is_sltiu ? $src1_value < $imm :
                         $is_lui ? {$imm[31:12], 12'b0} :
                         $is_auipc ? $pc + $imm :
                         $is_jal ? $pc + 4 :
                         $is_jalr ? $pc + 4 :
                         $is_srai ? {{32{$src1_value[31]}}, $src1_value} >> $imm[4:0] :
                         $is_slt ? ($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]} :
                         $is_slti ? ($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]} :
                         $is_sra ? {{32{$src1_value[31]}}, $src1_value} > $src2_value[4:0] :
                         32'bx ;
         
         
         //BRANCH INSTRUCTIONS 1
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         
         $valid_taken_branch = $valid && $taken_branch;
                  
      @2
         //BRANCH INSTRUCTIONS 2
         $br_target_pc[31:0] = $pc +$imm;
         
         
         //TESTBENCH
         *passed = |cpu/xreg[17]>>5$value == (1+2+3+4+5+6+7+8+9) ;
         
         
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.
   
  		 // Assert these to end simulation (before Makerchip cycle limit).
   		*passed = *cyc_cnt > 40;
  		 *failed = 1'b0;
   
  	 	// Macro instantiations for:
  		 //  o instruction memory
  		 //  o register file
   		//  o data memory
  		 //  o CPU visualization
  		 |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
 	  m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
		\SV
 	  endmodule

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/767a4550-0be0-4b4f-8da0-38d2d6508a04)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/12f11063-be93-44ef-abd9-bdd87acee8eb)

![image](https://github.com/V-Pranathi/RISC-V/assets/140998763/44afb3af-2d1b-40ce-8dc0-aae2f1e81104)

## <a name="6--Contributors"></a> 7. Contributors ##  
* V.Pranathi
* Kunal Ghosh

## <a name="7--References"></a> 6. References ##

* https://github.com/RISCV-MYTH-WORKSHOP/RISC-V-CPU-Core-using-TL-Verilog
* https://github.com/stevehoover/RISC-V_MYTH_Workshop/blob/master/risc-v_shell.tlv
