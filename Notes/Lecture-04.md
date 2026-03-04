# **Encoders, Decoders, Multiplexers, Programmable Logic Devices**

### **1. Encoders**

An encoder is a combinational circuit that performs the inverse operation of a decoder.

* **Structure:** A standard encoder has up to $2^n$ input lines and exactly $n$ output lines.
* **Functionality:** It takes active inputs and compresses them into a smaller binary code. For example, if you have an 8-to-3 encoder, and the input line representing the decimal number "5" is active, the encoder will output the 3-bit binary equivalent (101).
* **Priority Encoders:** In a standard encoder, if more than one input is active at the same time, the output is undefined or incorrect. To solve this, **priority encoders** are used. They assign a priority level to each input; if multiple inputs are active simultaneously, the encoder outputs the binary code of the input with the highest priority.
* **Use Cases:** Encoders are often used in systems where you need to reduce the number of wires needed to transmit information, such as converting a keypad press (multiple separate buttons) into a compact binary code for a processor to read.

<center>
    <img src="https://image.slideserve.com/1465475/encoders8-l.jpg" width="500">
</center>

---

### **2. Decoders**

A decoder is a combinational circuit used to decode encoded information.

* **Standard Decoder Characteristics:**
  * A standard decoder has $n$ data inputs and $2^{n}$ outputs, often referred to as an $n$-to-$2^{n}$ decoder.
  * **One-hot encoding:** For any specific input data combination, a unique output line is asserted (logic value 1) while all other outputs remain at 0.
  * *Example:* A 3-to-8 decoder can be used to decode a 3-bit instruction field to determine 1 out of 8 possible desired functions.
  * *2-to-4 Decoder Table:* For inputs $x_0$ and $x_1$, the outputs are activated sequentially ($Y_0$ for 00, $Y_1$ for 01, $Y_2$ for 10, and $Y_3$ for 11).

* **Specialized Decoders (Multiple Assertions):**
  * While standard decoders assert only one line, special decoders can assert multiple output lines simultaneously.
  * *Example:* A **BCD (binary-coded decimal) to seven-segment display decoder** takes a 4-bit BCD digit as input and outputs 7 bits (labeled a through g) corresponding to display segments. Any number from 0 to 9 can be displayed by selectively turning segments on and off.
  * If the input is 0100 (the digit 4), outputs b, c, f, and g are asserted (turned on).

<center>
    <img src="https://image2url.com/r2/default/images/1772637269143-922e51d7-2031-4727-97d7-5833e5d2501a.png" width="500">
</center>

---

### **3. Multiplexers (MUX)**

A multiplexer is a circuit that routes data from multiple sources to a single destination.

* **Structure:** A MUX consists of $2^{k}$ data inputs, $k$ select inputs, and exactly one output.
* **Functionality:** It passes the signal value from one of its data inputs directly to the output based solely on the binary value of the select signals.
* *Example Application:* Gating data from different sources, such as loading a register from one of four distinct sources using a 4-input MUX.
* **4-Input MUX Logic Equation:** Let inputs be $x_0, x_1, x_2, x_3$ and select lines be $w_0, w_1$. The output $z$ is defined as:

$$z = x_0\overline{w_0}\overline{w_1} + x_1\overline{w_0}w_1 + x_2w_0\overline{w_1} + x_3w_0w_1$$

* **Synthesizing Logic Functions using MUXes:**
  * MUXes can be utilized to synthesize complex logic functions.
  * By mapping some input variables to the MUX's select lines, the remaining variables (or fixed 0 and 1 values) can be fed into the data inputs to recreate a specific truth table. 
  * *Example:* A 3-variable truth table ($y_0, y_1, y_2$) can be synthesized on a 4-input MUX by using $y_1$ and $y_2$ as the select inputs, and determining the input pins based on the behavior of $y_0$ (e.g., passing $0, 1$, or $NOT(y_0)$).

<center>
    <img src="https://image2url.com/r2/default/images/1772639470799-4744babe-74c8-420b-ab64-fc0b84211cfc.png" width="500">
</center>
<center>
    <img src="https://image2url.com/r2/default/images/1772640117954-54cf2b77-db75-45bf-a87f-ba61dd8bbe2d.png" width="500">
</center>

---

### **4. Programmable Logic Devices (PLDs)**

A PLD is a customizable device capable of being programmed with switches to implement a wide variety of combinational logic functions.

* **General Structure:** Inputs (logic variables) pass through input buffers and inverters, feed into an **AND Plane**, and then route into an **OR Plane** to produce logical outputs.

* **Logic Theory:** Any combinational logic can be implemented via a **Sum of Products** (AND-OR implementation).

* To enable customization, programmable switches are placed within the AND and OR arrays.

**PLD Functionality Classification:** 
| Device Type | AND Array | OR Array |
| :--- | :--- | :--- |
| **Not Programmable** | Fixed | Fixed |
| **PROM** | Fixed | Programmable |
| **PAL** | Programmable | Fixed |
| **PLA** | Programmable | Programmable |

<center>
    <img src="https://image2url.com/r2/default/images/1772641168425-e01198f0-566a-4e89-8823-6b420010248b.png" width="500">
</center>

---

### **5. PLA vs. PAL**

The lecture specifically focuses on PLA and PAL architectures.

* **Programmable Logic Array (PLA):**
  * Both the AND and OR arrays are programmable.
  * Connections are established using programmable fuses.
  * Product terms generated in the AND plane can be shared among multiple output functions in the OR plane.

* **Programmable Array Logic (PAL):**
  * Features a programmable AND array, but a fixed OR array.
  * AND gates are permanently hardwired to specific OR gates.
  * The number of product terms per function is strictly limited by the number of AND gates connected to a specific OR gate.
  * Product terms cannot be shared amongst multiple output functions.

* **Trade-offs (PLA vs. PAL):**
  * **Flexibility:** PLA offers significantly more programming flexibility.
  * **Cost & Speed:** PAL is cheaper to implement because it requires fewer switches, and it achieves higher operating speeds due to its fixed connections.
  * **Modern Enhancements:** PAL circuits often integrate flip-flops and MUXes after the OR gate outputs to increase their flexibility.

<center>
    <img src="https://image2url.com/r2/default/images/1772641627838-9be16d7d-b0a1-459b-89b5-d0c8235f7077.png" width="500">
</center>

---

### **6. Complex Programmable Logic Devices (CPLDs)**

* **Structure:** CPLDs consist of multiple **PAL-like blocks** surrounded by I/O blocks.

* **Interconnectivity:** Connections between the individual PAL-like blocks are established by programming a central matrix of interconnect switches (Interconnection Wires).

* **Programming:** Configuration data is typically loaded into the device via a JTAG port. Computer-Aided Design (CAD) tools are frequently used to program large-scale CPLDs.

<center>
    <img src="https://image2url.com/r2/default/images/1772642063528-0811e4b5-18f7-4bdd-95af-16117071d6c8.png" width="500">
</center>

---

### **7. Programmable vs. Custom Devices**

There is an inherent trade-off between device programmability and constraints like cost, performance, and energy efficiency.

* **Programmable Devices (CPLDs and field-programmable gate arrays - FPGAs):**
  * Can be configured post-manufacturing to implement a **variety** of complex logic circuits.
  * Offer faster design cycles and less upfront development cost.

* **Custom Devices (ASICs - Application-Specific Integrated Circuits):**
  * Designed and specialized strictly for a **particular** operation (e.g., MPEG decoding).
  * They are tuned specifically for their designated task, resulting in higher performance and much better energy efficiency compared to programmable logic.
