# **Basic Processing Unit**

## 1. Overview of Hardware Components
The basic processing unit consists of several interconnected hardware blocks designed to fetch, decode, and execute instructions. The primary components include:

* **Register File:** A collection of general-purpose registers.
* **ALU (Arithmetic and Logic Unit):** Performs mathematical and logical operations.
* **Instruction Address Generator:** Includes the **Program Counter (PC)** and logic to determine the next instruction's address.
* **Instruction Register (IR):** Holds the current instruction being executed.
* **Control Circuitry:** Examines the IR and generates the signals required to coordinate all hardware components.
* **Processor-Memory Interface:** Manages data transfers between the processor and the main memory.

## 2. The Register File
General-purpose registers are implemented as a **register file**, which serves as a high-speed storage array for data currently being processed.

### Architecture and Access
* **Storage and Circuitry:** The file consists of an array of storage elements and access circuitry to read from or write into any register.
* **Support for Operands:** To support instructions with two source operands, the access circuitry can read two registers simultaneously. 
    * **Address Inputs A and B** (connected to the source register fields in the IR) select which registers to read.
    * **Address Input C** (connected to the IR's destination field) selects the register for writing.
* **Example (add R3, R4, R5):** In this operation, Address A is 4, Address B is 5, and Address C is 3.

### Dual-Ported Implementation
A memory unit with two output ports is considered **dual-ported**. There are two common ways to implement this:
1.  **True Ports:** A single set of registers with duplicate data paths and access circuitry allowing two simultaneous reads.
2.  **Two Copies:** Using two separate memory blocks, each containing a full copy of the register file.
    * Reading involves accessing one register from each copy.
    * Writing requires data to be written to the same register in **both** copies to maintain consistency.

## 3. Arithmetic and Logic Unit (ALU)
The ALU is the computational heart of the processor.

* **Operations:** It performs arithmetic (addition, subtraction, multiplication) and logic (bitwise AND, OR, XOR) functions.
* **Connectivity:** The register file and ALU are directly linked. Source operands read from the register file are fed into the ALU, and the computed result is eventually loaded back into the specified destination register.
* **Operand Selection (MuxB):** A multiplexer (**MuxB**) determines the second operand for the ALU.
    * **MuxB = 0:** Selects the output from Register File Port B (used for operations requiring only register operands).
    * **MuxB = 1:** Selects an **immediate value** from the IR (used for operations with a constant operand).
    * **Control Signal:** This signal is often the Least Significant Bit (LSB) of the opcode.

## 4. The Datapath and Instruction Stages
Instruction processing is divided into a **Front End** (fetch and decode) and a **Back End** (execute, memory access, and writeback).

### The Five Stages of Execution
The datapath uses **inter-stage registers** (RA, RB, RZ, RM, RY) to hold data between stages:

1.  **Stage 1 (Fetch):** The instruction is fetched from memory and loaded into the IR; the PC is incremented.
2.  **Stage 2 (Decode):** The control circuitry decodes the instruction, and source registers are read into inter-stage registers **RA** and **RB**.
3.  **Stage 3 (Execute):** The ALU performs the operation. The result is stored in **RZ**. For store instructions, data to be stored is moved to **RM**.
4.  **Stage 4 (Memory Access):** For Load/Store instructions, memory is accessed using the address in RZ. For others, the RZ value moves to **RY**.
5.  **Stage 5 (Writeback):** Data from RY is written into the destination register in the register file.

### Multiplexer Selection Table
Selection depends on the instruction type:

| Instruction Type | MuxB Selection (ALU Input B) | MuxY Selection (Writeback Source) |
| :--- | :--- | :--- |
| **Arithmetic (2 Regs)** | 0 (Register) | 0 (ALU Result) |
| **Arithmetic (1 Reg + Imm)** | 1 (Immediate) | 0 (ALU Result) |
| **Load** | 1 (Immediate Offset) | 1 (Memory Data) |
| **Store** | 1 (Immediate Offset) | X (No writeback) |
| **Subroutine Call** | X (Don't Care) | 2 (Return Address) |


## 5. Instruction Address Generator
This unit manages the PC and determines the address of the next instruction.

* **PC-Temp:** Holds the PC contents to save as a return address during subroutine calls.
* **MuxPC:** Selects between the incremented PC (input 1) or a branch/subroutine target (input 0 from RA).
* **MuxINC:** For branch instructions, it selects the **Branch Offset** (input 1); otherwise, it selects a constant **4** (input 0) to point to the next sequential instruction.
* **MuxMA:** Selects the address sent to memory. It selects input 1 (from PC) during an instruction fetch.

## 6. Instruction Encoding and Immediate Values
### Instruction Formats
Standardized formats simplify decoding:
* **Register-operand:** Specifies two source registers and one destination.
* **Immediate-operand:** Specifies one source, one destination, and an immediate value.
* **Call format:** Specifies an immediate value for the target address.

### Decoding Optimization
Reading source registers can proceed **simultaneously** with opcode decoding because source register addresses (bits 31-27 and 26-22) are located in the same positions for all instructions. If these registers are not needed for a specific instruction, subsequent stages simply ignore them.

### Immediate Field Extension
Immediate values (typically 16 bits) must be extended to 32 bits before being used by the ALU:
* **Logical Instructions:** Use **zero-padding** (extending with zeros).
* **Arithmetic/Branch Instructions:** Use **sign-extension** to preserve the numerical value.
