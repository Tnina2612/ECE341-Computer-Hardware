# **Sequential Logic: Latches and Flip-Flops**

## **1. Introduction to Sequential Logic**

### Combinational vs. Sequential Logic

To understand sequential circuits, it is necessary to contrast them with combinational logic.

* **Combinational Logic:** The outputs are uniquely defined solely by the current input combination. There is no concept of "time" or "history".
* **Sequential Logic:** The output depends not only on the current inputs but also on the **previous state** of the circuit. This implies the system possesses **memory**.

<center>
    <img src="https://image2url.com/r2/default/images/1769525306647-dff3be26-4a5a-44e4-b7d4-3176f186417f.png" width="400">
</center>

### The Clock Signal

Sequential circuits typically rely on a clock signal to synchronize state changes.

* **Level Triggering:** State changes can occur at any time as long as the clock signal is at a specific voltage level (e.g., active high during the "1" phase).
* **Edge Triggering:** State changes occur *only* during the transition of the clock signal.
  * **Positive Edge:** Transition from `0` to `1`.
  * **Negative Edge:** Transition from `1` to `0`.

<center>
    <img src="https://image2url.com/r2/default/images/1769525174171-1dec8747-f97b-47ae-8d2a-5c29a9f37a38.png" width="500">
</center>

### Latches vs. Flip-Flops

While both are basic memory elements used in sequential circuits, they differ in how they respond to timing signals.

* **Latch:** Watches inputs continuously and changes output at any time, irrespective of clock edges (often level-triggered).
* **Flip-Flop:** Samples inputs and changes output *only* at the edge of a controlling clock signal (edge-triggered).

## **2. Latches**

### The SR Latch

The SR (Set-Reset) latch is the most fundamental memory element.

* **Structure:** Composed of two cross-coupled NOR gates.
* **Inputs/Outputs:** It has a Set input ($S$), a Reset input ($R$), and two complementary outputs ($Q$ and $QN$ or $\overline{Q}$).

**Operational Truth Table:**
| S | R | Action | Next State ($Q$) | Note |
| :--- | :--- | :--- | :--- | :--- |
| **0** | **0** | **Hold** | $Q(t)$ | Acts as memory (retains previous state). |
| **0** | **1** | **Reset** | **0** | Forces $Q=0,\overline{Q}=1$. |
| **1** | **0** | **Set** | **1** | Forces $Q=1,\overline{Q}=0$. |
| **1** | **1** | **Invalid** | **X** | Typically not used; results in $Q=\overline{Q}=0$, violating logic complement rules. |

<center>
    <img src="https://image2url.com/r2/default/images/1769526040323-550c169d-284e-44ff-8bc0-f2f8646c191c.png" width="300">
</center>

### The Gated SR Latch

To control *when* the latch changes state, a clock (or "enable") input is added.

* **Circuit:** Standard SR latch with two AND gates at the input controlled by a Clock signal (`Clk`).
* **Operation:**
  * **When Clk = 0:** The inputs to the latch ($R',S'$) are forced to 0. The latch holds its state regardless of changes in external $S$ and $R$.
  * **When Clk = 1:** The external $S$ and $R$ inputs pass through to the latch, allowing it to Set or Reset normally.

<center>
    <img src="https://image2url.com/r2/default/images/1769526874754-31a4540b-7e6e-4097-9b5f-b2435841ea8a.png" width="500">
</center>

**Timing Diagram for Gated SR Latch**

<center>
    <img src="https://image2url.com/r2/default/images/1769527375565-2b24c8e9-943d-4a27-b225-a34c3c247f78.png" width="500">
</center>

### The Gated D Latch

The D (Data) latch solves the problem of the invalid $S=R=1$ state found in SR latches.

* **Structure:** It modifies the Gated SR latch by ensuring inputs are always complementary. A single input $D$ drives $S$, while $\overline{D}$ (inverted D) drives $R$.
* **Operation:**
  * **When Clk = 1 (Transparent Mode):** The output $Q$ follows the input $D$. If $D=1$, the latch sets; if $D=0$, the latch resets.
  * **When Clk = 0 (Hold Mode):** The output $Q$ retains its previous value regardless of changes in $D$.

<center>
    <img src="https://image2url.com/r2/default/images/1769527937551-776f8e73-61f0-4d14-9570-02ae2e6e632c.png" width="500">
</center>

### Limitations of Latches

Latches are **level-sensitive**, meaning the output responds immediately to input changes while the clock is high.

* **The Problem:** In circuits like counters or shift registers, this immediate propagation can cause race conditions or incorrect operation.
* **The Solution:** Use **Master-Slave Flip-Flops**, which isolate outputs from inputs except at specific clock transitions.

## **3. Flip-Flops**

### Master-Slave D Flip-Flop

This device is constructed using two Gated D Latches connected in series: a "Master" and a "Slave".

* **Operation (Negative Edge Triggered):**
  1. **When Clk = 1:** The Master is active and samples the input $D$. The Slave is inactive (holding previous data) because its clock input is inverted.
  2. **Transition (1→0):** The Master is disabled (locking in the value of $D$). The Slave becomes active and updates its output $Q$ to match the Master's stored value ($Q_m$).

* **Result:** Data transfer from Input → Output occurs only on the falling edge of the clock.
* D flip-flop is commonly used for temporary storage of data.

<center>
    <img src="https://image2url.com/r2/default/images/1769529402916-a39c0d4a-a4d3-41e3-8a3c-1185844d22d8.png" width="600">
</center>

### T Flip-Flop (Toggle)

The T Flip-Flop is designed to toggle its state, making it useful for counters.

* **Logic:**
  * **If T = 0:** Output holds state, $Q(t+1)=Q(t)$.
  * **If T = 1:** Output toggles, $Q(t+1)=\overline{Q}(t)$.
* **Construction:** Can be built using a D Flip-Flop with feedback logic implementing the function $D=T\oplus Q$ (XOR).

<center>
    <img src="https://image2url.com/r2/default/images/1769530352038-09c1eab5-4d93-4f95-881f-74d10908a069.png" width="500">
</center>

### JK Flip-Flop

The JK Flip-Flop is a versatile component that combines the behaviors of the SR and T flip-flops.

* **Inputs:** $J$ acts like Set ($S$), $K$ acts like Reset ($R$).
* The $J=K=1$ state creates the toggle action (T flip-flop), resolving the invalid state issue of the SR latch.
* JK flip-flop is  versatile; can be used both for data storage and building counters.

**Operational Truth Table:** 
| J | K | Action | Next State ($Q(t+1)$) |
| :--- | :--- | :--- | :--- |
| **0** | **0** | **Hold** | $Q(t)$ |
| **0** | **1** | **Reset** | 0 |
| **1** | **0** | **Set** | 1 |
| **1** | **1** | **Toggle** | $\overline{Q}(t)$ |


<center>
    <img src="https://image2url.com/r2/default/images/1769531716531-f40985e3-b682-42d8-b805-c9571be19ca5.png" width="500">
</center>

### Summary of Triggering Mechanisms

* **Latches:** Level-triggered (Sample inputs while `Clk` is high).
* **Flip-Flops:** Edge-triggered (Sample inputs only at the 0→1 or 1→0 transition).
