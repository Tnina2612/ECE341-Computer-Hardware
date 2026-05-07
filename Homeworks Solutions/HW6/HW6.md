# ECE341 HOMEWORK 6

## Problem 1

### 1. The Storage (Registers)
We will need **four 8-bit registers**. In Logisim, place four "Register" components from the Memory library. 
* **Data Bit Width:** 8
* **Trigger:** "Rising Edge"

### 2. Read Logic (Output)
Since we have two independent "Read data" outputs, we need two identical circuits to select which register's value is sent to the output.
* **Components:** Two **8-bit 4-to-1 Multiplexers**.
* **Wiring:** * Connect the output of Register 0 to the first input (0) of both Muxes.
    * Connect Register 1 to the second input (1) of both Muxes, and so on.
    * Select Lines: Connect the 2-bit "Read register number 1" to the select line of the first Mux. Connect "Read register number 2" to the select line of the second Mux.

### 3. Write Logic (Input)
Writing is more selective. We only want to update one register at a time, and only when the `Write` (Enable) signal is high.
* **Component:** A **2-to-4 Decoder**.
* **Wiring:**
    * **Input:** Connect the 2-bit "Write register" address to the decoder input.
    * **Enable:** Connect the 1-bit "Write" signal to the "Enable" pin of the decoder.
    * **Outputs:** The four outputs of the decoder will act as the **Clock Enable (WE)** signals for four registers.
    * **Data & Clock:** All four registers share the same 8-bit "Write data" input and the same "Clock" signal. However, a register will only save the data if its specific Enable pin (from the decoder) is active when the clock pulses.

---

## Problem 2

### 1. Component Configuration
First, place a RAM component (from the "Memory" library) and set these properties:
* **Address Bit Width:** 15 (Because $2^{15} = 32,768$, which is 32 KB).
* **Data Bit Width:** 8.
* **Data Interface:** Keep it as "One synchronous load/store port" (Standard).

### 2. Wiring the Controls
Mapping the pins from the diagram to the Logisim RAM component:

| Diagram Label | Logisim RAM Pin | Function |
| :--- | :--- | :--- |
| **address** | **A** | Connect 15-bit address bus here. |
| **chip select** | **sel** | Enables the entire block when high. |
| **output enable** | **ld** | When high, the RAM drives data out to the `D` pin. |
| **write enable** | **str** | When high, data is written from the `D` pin into memory on a clock edge. |
| **(System Clock)** | **clk** | Required for the `str` (write) operation to execute. |

### 3. Separating Data In and Data Out

1.  **Data In:** Connect 8-bit `data in` input pin directly to the RAM's D pin.
2.  **Data Out Buffer:**
    * **Input:** Connect to the RAM's D pin node.
    * **Output:** Connect to `data out` output pin.
    * **Select (Control Pin):** Connect this to the **output enable** (OE) signal. 
    * *Result:* When `output enable` is low, the `data out` pin will be "High-Z" (disconnected).

### 4. Experimental Procedures

#### A. The Write Experiment
1.  Set **address** to a specific location (e.g., `0x0001`).
2.  Set **data in** to a test value (e.g., `0xAB`).
3.  Set **chip select** to 1 and **write enable** to 1.
4.  Set **output enable** to 0 (to keep the output bus clean).
5.  **Pulse the clock.** The value `0xAB` is now stored at address `0x0001`.

#### B. The Read Experiment
1.  Keep **address** at `0x0001`.
2.  Set **write enable** to 0 (to prevent overwriting).
3.  Set **chip select** to 1.
4.  Set **output enable** to 1.
5.  **Observe:** The data out pin should now display `0xAB`.

---

## Problem 3

To design an **8 KB RAM** using the specific blocks provided ($2 \times 1\text{ KB}$ and $5 \times 2\text{ KB}$), we will use a total of **5 physical blocks**: both 1 KB blocks and three of the 2 KB blocks.

An 8 KB memory space requires **13 address bits** ($2^{13} = 8,192$ bytes), ranging from `0x0000` to `0x1FFF`.

### 1. Address Range Specification
To map these blocks into a continuous 8 KB space, we divide the memory as follows:

| Block Type | Block Name | Address Range (Hex) | Address Range (Decimal) | Selection Logic (A12–A10) |
| :--- | :--- | :--- | :--- | :--- |
| **1 KB RAM** | Block A | `0x0000` – `0x03FF` | 0 – 1023 | `000` |
| **1 KB RAM** | Block B | `0x0400` – `0x07FF` | 1024 – 2047 | `001` |
| **2 KB RAM** | Block C | `0x0800` – `0x0FFF` | 2048 – 4095 | `01x` |
| **2 KB RAM** | Block D | `0x1000` – `0x17FF` | 4096 – 6143 | `10x` |
| **2 KB RAM** | Block E | `0x1800` – `0x1FFF` | 6144 – 8191 | `11x` |

### 2. Implementation

#### A. The Address Splitter
We need a 13-bit Address Input. Use a Splitter to divide these bits for decoding:
* **Low-order bits:** Used to index inside the RAM blocks.
* **High-order bits:** Used to "Select" which RAM block is active.

#### B. Decoding Logic (Chip Select)
We need to enable only one RAM block at a time based on the MSB of the address:

1.  **Primary Decoder (3-bit to 8-line):** Connect address bits **A12, A11, and A10** to a decoder.
    * **Output 0:** Connect to the `sel` (Select) pin of **1 KB Block A**.
    * **Output 1:** Connect to the `sel` (Select) pin of **1 KB Block B**.
2.  **Handling the 2 KB Blocks:** Since a 2 KB block covers two 1 KB "slots" in a 3-bit decoder, we must combine the decoder outputs using **OR gates**:
    * **Outputs 2 OR 3:** Connect to the `sel` pin of **2 KB Block C**.
    * **Outputs 4 OR 5:** Connect to the `sel` pin of **2 KB Block D**.
    * **Outputs 6 OR 7:** Connect to the `sel` pin of **2 KB Block E**.

#### C. Wiring the Data and Internal Addresses
* **Address Pins:**
    * Connect address bits **A9–A0** (10 bits) to the address inputs of the **1 KB blocks**.
    * Connect address bits **A10–A0** (11 bits) to the address inputs of the **2 KB blocks**.
* **Data Bus:** All RAM blocks share the same **8-bit Data Input** and **8-bit Data Output** (use Tri-state buffers if your Logisim version requires them, though Logisim RAM blocks usually handle this internally when the `sel` pin is low).
* **Control Signals:** Connect the `Clock`, `LD` (Load/Read), and `str` (Store/Write) signals to all blocks in parallel.

---

## Problem 4

To expand your memory capacity from 8 KB to 32 KB, we will need four 8 KB RAM blocks. A 32 KB memory space requires a 15-bit address bus ($2^{15} = 32,768$ bytes).

### 1. Address Mapping & Decoding
We use the 2 most significant bits (**A14 and A13**) of the 15-bit address to select which of the four 8 KB blocks is active. The remaining 13 bits (**A12–A0**) are sent to every block to index the specific byte.

| Block Name | Address Range (Hex) | Address Range (Decimal) | A14, A13 (Binary) |
| :--- | :--- | :--- | :--- |
| **Block 0** | `0x0000` – `0x1FFF` | 0 – 8,191 | `00` |
| **Block 1** | `0x2000` – `0x3FFF` | 8,192 – 16,383 | `01` |
| **Block 2** | `0x4000` – `0x5FFF` | 16,384 – 24,575 | `10` |
| **Block 3** | `0x6000` – `0x7FFF` | 24,576 – 32,767 | `11` |

### 2. Implementation

#### A. The Address Splitter
* Create a 15-bit Input Pin for your address.
* Attach a Splitter with "Fan out" 2 and "Bit Width In" 15.
* Set the first arm to bits 0–12 (these are your 13 internal address bits).
* Set the second arm to bits 13–14 (these are your 2 selection bits).

#### B. The Selection Logic
* Place a 2-to-4 Decoder.
* Connect the 2-bit selection arm from your splitter to the decoder input.
* Connect Output 0 of the decoder to the `sel` (Select) pin of RAM Block 0, Output 1 to Block 1, and so on. 

#### C. The Bidirectional Data Bus
Logisim RAM blocks have a single D (Data) pin that is naturally bidirectional. To make this work as a unified 32 KB unit:

1.  **Parallel Connection:** Wire the D pins of all four RAM blocks together into a single 8-bit bus.
2.  **External Interface:** Connect this common 8-bit bus to your circuit's main data bus.
3.  **The "Output Enable" (ld) Signal:** * Connect your global Read/Load signal to the `ld` pin of all four blocks.
    * When `ld` is 1 and a block is selected via `sel`, that block will drive its data onto the bus.
    * When `ld` is 0, the blocks act as inputs (High-Impedance state), allowing we to write data *to* them.
4.  **The "Store" (str) Signal:** * Connect your global Write/Store signal to the `str` pin of all blocks. Data is written only if `sel` is also high.
