# **Registers, Counters, and Finite State Machines**

## **1. Registers**

### Definition and Functionality

* **Basic Unit:** A flip-flop stores one bit of information.
* **Register Definition:** A set of  flip-flops used together to store  bits of data is called an **n-bit register**.
* **Synchronization:** All flip-flops within a register are synchronized by a common clock.
* **Operation:** Data is loaded and stored into all flip-flops simultaneously.
* **Usage:** Commonly used for temporary storage of data output from arithmetic circuits.

<center>
    <img src="https://image2url.com/r2/default/images/1769533628498-4bd2a728-1fdc-4881-bfa8-ce38788d5ac5.png" width="500">
</center>

### Shift Registers

A shift register allows its contents to be shifted one bit position at a time (left, right, or both).

* **Shift Right Logic:** At each positive clock edge, the contents of flip-flop  are shifted to the next flip-flop .
* **Hardware constraint:** Standard gated latches are unsuitable for shift registers because they offer no control over the number of shifts per clock cycle; edge-triggered flip-flops are required.
* **Example (4-bit Shift Right):**
  * **Input Data:** 1001.
  * **Operation:** The data enters the first flip-flop ($F_1$) bit-by-bit. On each clock edge, the bit moves to the next flip-flop ($F_1\rightarrow F_2\rightarrow F_3\rightarrow F_4$). After 4 clock cycles, the full 4-bit word is stored across the register.

<center>
    <img src="https://image2url.com/r2/default/images/1769533965830-cf37fd65-e1b9-4641-810a-7eccc32641e1.png" width="500">
</center>

### Parallel Access Shift Registers

Data transfer in computer systems occurs in two modes:

1. **Serial:** Transfer is 1-bit at a time (shifted one position each cycle).
2. **Parallel:** Transfer is n-bits (n > 1) simultaneously in a single clock cycle.

<center>
    <img src="https://www.build-electronic-circuits.com/wp-content/uploads/2023/02/pipo.png" width="500">
</center>

**Circuit Implementation:**
A Parallel Access Shift Register uses logic gates (AND/OR combination) and a control signal (`Shift/Load`) to select the input source for the flip-flops.

* **Parallel Load (`Shift/Load` = 1):** The circuit selects the **Parallel input** lines. All  bits are loaded into the register at once.
* **Serial Shift (`Shift/Load` = 0):** The circuit selects the **Serial input** (or the output of the previous flip-flop). The register shifts data bit-by-bit.

<center>
    <img src="https://image2url.com/r2/default/images/1769534681707-52df9709-0772-40d6-b76b-bb267d829e59.png" width="500">
</center>

## **2. Counters**

### Overview

* **Definition:** Arithmetic circuits used for counting (incrementing or decrementing by 1 per cycle).
* **Implementation:** Often implemented using **T flip-flops** because their toggle feature is naturally suited for counting.
* **Applications:**
  * Counting event occurrences (e.g., number of instructions).
  * Tracking elapsed time between events.
  * Generating timing signals or frequency dividers.

### 3-bit Up-Counter (Ripple Counter)

* **Behavior:**
  * **LSB ($x_0$):** Toggles at every increment (every clock cycle).
  * **Bit 1 ($x_1$):** Toggles when $x_0$ transitions from $1\rightarrow 0$ (half the rate of toggling $x_0$).
  * **Bit 2 ($x_2$):** Toggles when $x_1$ transitions from $1\rightarrow 0$ (half the rate of toggling $x_1$).
* **Circuit:** The clock feeds only the first flip-flop. The output of one flip-flop acts as the clock for the next.

<center>
    <img src="https://image2url.com/r2/default/images/1769563700450-8d580e55-e89b-497a-9f32-b3c130016e28.png" width="500">
</center>

### 3-bit Down-Counter

* **Behavior:**
  * **LSB ($x_0$):** Toggles at every decrement.
  * **Bit 1 ($x_1$):** Toggles when $x_0$ transitions from $0\rightarrow 1$.
  * **Bit 2 ($x_2$):** Toggles when $x_1$ transitions from $0\rightarrow 1$.

* **Converting Up-Counter to Down-Counter:**
To change direction, you must modify the triggering logic using one of two methods:

  1. Replace positive edge-triggered T flip-flops with **negative edge-triggered** T flip-flops.
  2. **OR:** Connect the $Q$ outputs (instead of $\overline{Q}$ outputs) from the previous flip-flops to the clock inputs of the next flip-flops.

### Asynchronous (Ripple) Counters

* The counters described above are **Asynchronous** (or Ripple Counters) because the input clock connects only to the first flip-flop. Clocks for subsequent stages are derived from previous outputs.
* **Disadvantage:** They are slow due to the "ripple" effect - propagation delays cascade through the chain of flip-flops, limiting operation speed.
* **Solution:** Use Synchronous sequential circuits (Finite State Machines).

## **3. Finite State Machines (FSM)**

### Definition

* A **Sequential Circuit** where outputs depend on both **present inputs** and the **sequence of previous inputs** (stored as "state").
* **FSM Model Components:**
  * Combinational Logic (calculates Next State and Outputs).
  * Delay Elements/Flip-flops (store Present State).

<center>
    <img src="https://image2url.com/r2/default/images/1769565890064-fde4a1b3-de05-46d1-a914-8b99d59098e5.png" width="500">
</center>

### Synthesis Steps

Designing an FSM involves 6 steps:
1. **State Diagram/Table:** Depict transitions based on input patterns.
2. **Determine Flip-Flops:** Decide the number and type needed.
3. **State Assignment:** Assign binary values to each state.
4. **State-Assigned Table:** Create the table using binary values.
5. **Logic Expressions:** Derive logic for Next State and Outputs by K-map or SoP.
6. **Implementation:** Build the circuit from the derived expression.

## **4. Design Example: Up/Down Counter**

**Problem Statement:** Design a Mod-4 counter using D flip-flops that counts up or down based on input $x$ and outputs $z=1$ when the count is 2.

* **Inputs/Outputs:**
  * Input $x$: 0 = Count Up, 1 = Count Down.
  * Output $z$: 1 if State is S2 (Count 2), otherwise 0.
* **States:** 4 states ($S_0,S_1,S_2,S_3$).

**Design Process:**

1. **State Assignment:**
   * $S_0 = 00$
   * $S_1 = 01$
   * $S_2 = 10$ (Target for Output $z = 1$)
   * $S_3 = 11$

<center>
    <img src="https://image2url.com/r2/default/images/1769574147043-d2de4729-59fc-4db9-a531-39c4ae7ba36d.png" width="500">
</center>

2. **Flip-Flop Requirement:** 4 states require **2 D Flip-Flops**.

<center>
    <img src="https://image2url.com/r2/default/images/1769568784892-21f8dc90-46b6-42b9-bd04-8f7e17b49927.png" width="500">
</center>

3. **Next State Logic (Derived from Table):**
   * $Y_2$ (Next State MSB): $Y_2=y_2\oplus y_1\oplus x$.
   * $Y_1$ (Next State LSB): $Y_1=\overline{y_1}$ (The LSB toggles every cycle regardless of direction).

4. **Output Logic:**
   * The boolean logic for state 10 is $z=y_2\overline{y_1}$.

<center>
    <img src="https://image2url.com/r2/default/images/1769568706800-cb5d1787-236a-41e3-a299-0285b60f2886.png" width="500">
</center>