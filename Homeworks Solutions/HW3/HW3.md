# ECE341 HOMEWORK 3

## Problem 1

### Truth Table

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/1e366162-9330-4ada-9b19-9ded33cb6618.jpg" width="500">
</center>

### Karnaugh Maps and Derived Expressions

For each K-map, the bits marked with diagonal lines correspond to the **first expression** shown below the K-map, while the bits **without** diagonal lines correspond to the **second expression** shown below it.

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/b380e25a-9e47-4408-b2b9-29423d7e49d6.jpg" width="500">
</center>
<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/b38e02ff-6515-4dc0-8f59-921cf0635c19.jpg" width="500">
</center>

## Problem 2

Add `NOT` after each `OR` (`NOR` literally) in problem 1 and we are done.

## Problem 3

### Truth Table, K-map and Derived Expressions

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/bf82f3e5-c602-40d6-9345-62061f33e50d.jpg" width="500">
</center>

## Problem 4

We use one decoder to decide "which group of four" is active, and the other decoders to select the specific output within that group. Since a 4-bit input ($A_3, A_2, A_1, A_0$) has 16 possible states, we need five 2-to-4 decoders in total.

To decode 4 bits ($A_3, A_2, A_1, A_0$ where $A_3$ is the Most Significant Bit):
* The "Master" Decoder: Uses the two highest bits ($A_3, A_2$) to enable one of the four "Slave" decoders.
* The "Slave" Decoders: All four use the two lowest bits ($A_1, A_0$) to drive the final 16 outputs ($0-15$).

We now have 16 output pins across the four slave decoders:
* Slave 1 outputs represent Binary 0000 to 0011 (Decimal 0–3).
* Slave 2 outputs represent Binary 0100 to 0111 (Decimal 4–7).
* Slave 3 outputs represent Binary 1000 to 1011 (Decimal 8–11).
* Slave 4 outputs represent Binary 1100 to 1111 (Decimal 12–15).

## Problem 5

**First Stage (Select Input $S_0$):** Use two 2:1 Muxes.
* Mux 1 handles data inputs $X_0$ and $X_1$.
* Mux 2 handles data inputs $X_2$ and $X_3$.
* Both share the same first select bit ($S_0$).

**Second Stage (Select Input $S_1$):** Use one final 2:1 Mux.
* This Mux takes the outputs from the first two Muxes as its data inputs.
* It uses the second select bit ($S_1$) to decide which "pair" of inputs to pass to the final output $Q$.

That design works because:
* When $S_1$ is 0, the 3rd Mux only cares about the "top half" ($X_0$ and $X_1$). $S_0$ then chooses between those two.
* When $S_1$ is 1, the 3rd Mux switches to the "bottom half" ($X_2$ and $X_3$). $S_0$ then chooses between those two.

## Problem 6

### 4:1 Multiplexer (Mux)

A 4:1 Mux uses 2 select bits ($S_1, S_0$) to choose one of four data inputs ($X_0, X_1, X_2, X_3$).

#### Boolean Expression

$$Q = (X_0 \cdot \bar{S_1} \cdot \bar{S_0}) + (X_1 \cdot \bar{S_1} \cdot S_0) + (X_2 \cdot S_1 \cdot \bar{S_0}) + (X_3 \cdot S_1 \cdot S_0)$$

### 1:4 Demultiplexer (Demux)

A Demux performs the reverse: it takes one data input ($X$) and routes it to one of four outputs ($Y_0, Y_1, Y_2, Y_3$) based on the select lines ($S_1, S_0$).

#### Boolean Expressions

* $Y_0 = X \cdot \bar{S_1} \cdot \bar{S_0}$
* $Y_1 = X \cdot \bar{S_1} \cdot S_0$
* $Y_2 = X \cdot S_1 \cdot \bar{S_0}$
* $Y_3 = X \cdot S_1 \cdot S_0$

### Enhancement for Expansion

To easily expand these into 8:1 or 1:8 versions, we add an **Enable (EN)** pin to the blocks. This follows the same hierarchical logic used in decoders.

#### For the Mux (8:1 Expansion)

* **The Logic:** Add the $EN$ signal as a 4th input to every AND gate in your 4:1 Mux.
* **The Expansion:** To make an 8:1 Mux:
  1. Use two 4:1 Mux blocks.
  2. Connect a 1:2 Decoder (or a simple NOT gate) to the **Enable** pins using a 3rd select bit ($S_2$).
  3. Feed the outputs of both 4:1 Muxes into a final **2-input OR gate**.

#### For the Demux (1:8 Expansion)

* **The Logic:** Similarly, add an $EN$ signal as a 4th input to the AND gates.
* **The Expansion:** To make a 1:8 Demux:
  1. Use the 3rd select bit ($S_2$) to enable either "Demux Block A" (outputs 0-3) or "Demux Block B" (outputs 4-7).
  2. The data input $D$ is sent to both blocks, but only the enabled block will pass the signal through.

## Problem 7

We use output of the first multiplexer in the chip 74153 for part (a), and the second multiplexer for part (b).

### Part (a)

#### Truth Table

| A | B | C | Y |
|---|---|---|---|
| 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 |

#### Transform the truth table to use B and C as inputs

| B | C | Y |
|---|---|---|
| 0 | 0 | A̅ |
| 0 | 1 | 0 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

### Part (b)

#### Truth Table

| A | B | C | Y |
|---|---|---|---|
| 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 |

#### Transform the truth table to use B and C as inputs

| B | C | Y |
|---|---|---|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |
