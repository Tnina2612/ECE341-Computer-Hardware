# **Computer Organization, Performance Metrics, and Basic Processing Unit**

## 1. Basic Organization and Information
Computers process two primary types of information: **instructions** and **data**.

* **Instructions:** These are strings of binary digits (bits) that govern computer activity. They specify arithmetic and logic operations and control the transfer of information between different units.
* **Programs:** A list of instructions working together to perform a specific task.
* **Data:** Numbers and characters used as operands by instructions, also encoded as binary strings.

### Functional Units of a Computer
A computer consists of five main functional units: **Input**, **Memory**, **Arithmetic and Logic (ALU)**, **Control**, and **Output**. The ALU and Control units together constitute the **Processor**. These units are linked via an interconnection network.

* **Input and Output (I/O) Units:** These allow the operator to interact with the computer. Input units (e.g., keyboard, mouse) accept coded information, while output units (e.g., printer, graphic display) send results to the outside world. Some devices, like mobile touch screens, handle both.
* **Memory Unit:** Stores programs and data. It is divided into two classes:
    * **Primary Memory (Main Memory):** Fast, volatile storage where programs must reside during execution. It is accessed at word granularities using an address and a control command (read/write). A **Cache** is a faster memory that keeps a subset of main memory for immediate processor use.
    * **Secondary Storage:** Non-volatile, less expensive, and significantly slower than primary memory.
* **Arithmetic/Logic Unit (ALU):** Executes operations such as addition, subtraction, and multiplication. Operands are fetched from memory or registers, and results are stored back for immediate re-use or long-term storage.
* **Control Unit:** Often called the "nerve center," it coordinates the activities of all other units by generating timing signals. It is physically distributed across the computer via control wires rather than being one centralized block.

## 2. Computer Performance
Performance is the traditional metric for evaluating computers and is measured in terms of a program's **execution time**.

### The Basic Performance Equation
The time ($T$) required to execute a program is determined by the following formula:
$$T = \frac{N \times CPI}{R}$$

* **N (Number of Instructions):** The count of **dynamic instructions** executed (e.g., a loop of 8 instructions running 5 times counts as 40 instructions). This depends on the Instruction Set Architecture (ISA).
* **CPI (Cycles Per Instruction):** The average number of clock cycles required to execute one instruction. This depends on circuit speed, logic complexity, and memory access latency.
* **R (Clock Rate):** The number of clock cycles per second (Hertz). The **clock period** is the length of one cycle ($R = 1 / \text{clock period}$).

### Strategies to Increase Performance
* **Hardware Improvements:** Using faster logic/arithmetic circuits and employing caches to reduce memory latency.
* **Increasing Parallelism:** * **Instruction-Level Parallelism:** Executing multiple instructions at once or using **pipelining** (fetching the next instruction while executing the current one).
    * **Superscalar Execution:** Using multiple ALUs to execute instructions concurrently, which can result in a CPI $< 1$.
    * **Thread-Level Parallelism:** Running multiple programs simultaneously via multi-core processors.
* **Clock Speed Advances:** Improving manufacturing technology to create faster transistors and deeper pipelines (though deeper pipelines may increase CPI).
* **Compiler Technology:** Optimizing the conversion of high-level programs into the "least expensive" set of machine instructions to minimize $N \times CPI$.

### ISA Tradeoffs: RISC vs. CISC
* **CISC (Complex Instruction Set Computers):** Uses fewer, more complex instructions (lower $N$) that take longer to execute (higher $CPI$ or lower $R$).
* **RISC (Reduced Instruction Set Computers):** Uses simpler instructions that execute quickly (lower $CPI$ or higher $R$), though more instructions are needed for the same task (higher $N$).

## 3. The Basic Processing Unit
A typical task consists of a sequence of instructions that the processor carries out through rudimentary operations: **Fetching**, **Decoding**, and **Executing**.

### Processor Registers
The processor contains several internal registers to manage data and instructions:
* **Instruction Register (IR):** Holds the instruction currently being executed.
* **Program Counter (PC):** Contains the address of the next instruction to be fetched.
* **General-Purpose Registers ($R_0$ to $R_{n-1}$):** Form a **register file** to hold operands currently being processed.
* **Memory Address Register (MAR):** Holds the address of the memory word being read or written.
* **Memory Data Register (MDR):** Holds the actual data being transferred to or from memory.

> **Key Concept:** You cannot access memory data directly; you must use the interface registers (MAR and MDR).

### Instruction Set Architecture (ISA) & RTN
ISA specifies the four types of operations a computer can perform: data transfers, arithmetic/logic operations, program sequencing (branches), and I/O transfers. **Register Transfer Notation (RTN)** describes these data transfers:
* **$[LOC]$:** Denotes the contents of location $LOC$.
* **$R2 \leftarrow [LOC]$:** Move contents of memory location $LOC$ to register $R2$.
* **$R4 \leftarrow [R2] + [R3]$:** Add contents of $R2$ and $R3$, then place the result in $R4$.

## 4. RISC Instruction Processing
RISC architectures have two main characteristics: every instruction fits in exactly one memory word, and they use a **load/store architecture** (only Load and Store instructions can access memory).

### The 5-Stage Pipeline
Instruction processing in a RISC processor is typically broken into five stages:
1.  **Fetch:** Get instruction from memory address in PC, load it into IR, and increment PC by 4.
2.  **Decode:** Determine instruction type and operands; read source registers from the register file.
3.  **Execute:** Perform an ALU operation (e.g., addition or address calculation for memory instructions).
4.  **Memory Access:** Read from or write to memory if required.
5.  **Writeback:** Store the result back into the destination register.

### Specific Instruction Examples
* **Load ($R5 \leftarrow [[R7] + X]$):** Fetches the instruction, decodes it, adds immediate value $X$ to $R7$ to get an address, reads memory at that address, and writes the data to $R5$.
* **Arithmetic ($R3 \leftarrow [R4] + [R5]$):** Fetches, decodes, reads $R4$ and $R5$, adds them in the ALU, and writes the result to $R3$.
* **Store ($[R8] + X \leftarrow [R6]$):** Fetches, decodes, calculates the target address ($R8 + X$), and sends the contents of $R6$ to memory. There is no writeback to a register.


## 5. Hardware Components
* **Register File:** An array of storage elements with access circuitry. To support RISC, it is often **dual-ported**, allowing two registers to be read simultaneously (Address A and B) while one is being written (Address C).
* **ALU Connection:** The ALU receives one input directly from a register and a second input through a **multiplexer (MuxB)**. MuxB selects between a second register operand or an **immediate value** from the instruction word.
* **Pipelining Hardware:** Multi-stage hardware uses inter-stage registers to hold results from one clock cycle to be used as inputs for the next. The clock period must be longer than the delay of the slowest logic circuit in the chain.
