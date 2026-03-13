# ECE341 HOMEWORK 4

## Problem 1

### Half Adder

* Sum: $S = A \oplus B$
* Carry out: $C_{\text{out}} = AB$

### Full Adder

* Sum: $S = A \oplus B \oplus C_{\text{in}}$
* Carry out: $C_{\text{out}} = AB + (A \oplus B)C_{\text{in}}$

## Problem 2

Since $A - B = A + \sim B + 1$, we can add NOT before inputs $B_i$'s of Full Adder blocks and set the carry in to be $1$. 

## Problem 3

Set $C$ to be $C_{\text{in}}$ and set input of each Full Adder block to be XOR between $C$ and $B_i$. If $C = 0$, each input is $B_i$ itself since $B_i \oplus 0 = B_i$. If $C = 1$, each input is $\sim B_i$ since $B_i \oplus 1 = \sim B_i$.

## Problem 4

Design from the expressions of CLA.

## Problem 5

### Phase 1: The Initial Addition

1. Place the first **4-bit Adder**.
2. Connect 4-bit input pins **A** and **B** to the inputs of this adder.
3. The output of this adder is "Binary Sum."

### Phase 2: The "Check > 9" Logic

We need to detect if the binary sum is greater than 9. Since BCD only goes up to 9 ($1001_2$), we trigger a correction if:

* There is a **Carry Out ($C_{\text{out}}$)** from the first adder (meaning the sum is $\ge 16$).
* OR the sum is **10 or 11** (Binary $101x$).
* OR the sum is **12, 13, 14, or 15** (Binary $11xx$).

**Wiring the Logic:**

1. Connect a **Splitter** to the output of the first adder to get bits $S_3, S_2, S_1, S_0$ (where $S_3$ is the MSB).
2. Place an **AND gate** to check for $S_3$ AND $S_2$ (detects 12–15).
3. Place another **AND gate** to check for $S_3$ AND $S_1$ (detects 10–11).
4. Place a 3-input **OR gate**. Connect:
   * Input 1: The $C_{\text{out}}$ from the first adder.
   * Input 2: Output of the $(S_3 \cdot S_2)$ AND gate.
   * Input 3: Output of the $(S_3 \cdot S_1)$ AND gate.
5. The output of this OR gate is **BCD Carry Out**.

### Phase 3: The Correction Addition

1. Place the second **4-bit Adder**.
2. **Input 1:** Connect the "Binary Sum" (the output from the first adder).
3. **Input 2 (The Correction Value):** This needs to be **6 ($0110_2$)** if the OR gate is HIGH, or **0 ($0000_2$)** if it's LOW.
   * Connect the OR gate output to the middle two bits (bits 1 and 2) of the second adder’s input.
   * Connect the outer bits (bits 0 and 3) to a **Ground** (0).
4. The output of this second adder is final **BCD Sum**.

## Problem 6

### Component Configuration (74181)

To make the **74181** act as a standard binary adder, you must set its control pins correctly. For both ICs:

* **Mode Control ($M$):** Set to **Low (0)** for Arithmetic mode.
* **Select Inputs ($S_3, S_2, S_1, S_0$):** Set to **$1001$** (High, Low, Low, High). This selects the function $A$ plus $B$.

### Pin Mapping & Wiring

We will use two 74181s (one for bits 0-3, one for bits 4-7) and one 74182.

#### IC 1: 74181 (Lower 4 bits, $A_{0-3}, B_{0-3}$)

* **Inputs:** Connect $A_{0-3}$ and $B_{0-3}$ to the data inputs.
* **Outputs:** $F_{0-3}$ are sum bits $S_{0-3}$.
* **Carry-In ($C_n$):** Connect to the external carry-in ($C_{in}$) of 8-bit adder.
* **Generate ($\overline{G}$):** Connect to the $\overline{G}_0$ input of the **74182**.
* **Propagate ($\overline{P}$):** Connect to the $\overline{P}_0$ input of the **74182**.

#### IC 2: 74181 (Upper 4 bits, $A_{4-7}, B_{4-7}$)

* **Inputs:** Connect $A_{4-7}$ and $B_{4-7}$ to the data inputs.
* **Outputs:** $F_{0-3}$ are sum bits $S_{4-7}$.
* **Carry-In ($C_n$):** Connect to the **$C_{n+x}$** output of the **74182**.
* **Generate ($\overline{G}$):** Connect to the $\overline{G}_1$ input of the **74182**.
* **Propagate ($\overline{P}$):** Connect to the $\overline{P}_1$ input of the **74182**.

#### IC 3: 74182 (CLA Generator)

* **$C_n$ Input:** Connect to the global external carry-in ($C_{in}$).
* **$\overline{G}_0, \overline{P}_0$:** From the Lower 74181.
* **$\overline{G}_1, \overline{P}_1$:** From the Upper 74181.
* **$C_{n+x}$ Output:** Feeds the Carry-In ($C_n$) of the Upper 74181.
* **$C_{n+y}$ Output:** This would be the Carry-Out ($C_{out}$) for the full 8-bit result.
