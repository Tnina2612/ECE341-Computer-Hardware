# **Digital Logic Design**

## 1. Logic Gates

Logic gates are the physical building blocks of digital electronics. They are electronic devices (built from transistors) that perform a specific Boolean function: taking one or more binary inputs (0 or 1) and producing a single binary output.

### A. Basic Gates - Fundamental operators of Boolean algebra

* **`AND`:** Output is 1 only if **all** inputs are 1.
* **`OR`:** Output is 1 if **at least one** input is 1.
* **`NOT` (Inverter):** Output is the inverse of the input (1 becomes 0, 0 becomes 1).

### B. Universal Gates - Used to construct any other logic gate or circuit

* **`NAND` (Not `AND`):** Output is 0 only if all inputs are 1 (Functionally an `AND` gate followed by a `NOT` gate).
* **`NOR` (Not `OR`):** Output is 1 only if all inputs are 0.

### C. Special Gates

* **`XOR` (Exclusive `OR`):** Output is 1 if inputs are **different** (one is 1, the other is 0). Used heavily in adders and parity checks.
* **`XNOR` (Exclusive `NOR`):** Output is 1 if inputs are the **same**.

<center>
    <img src="https://i.pinimg.com/originals/c8/b8/c6/c8b8c60ce6fd7e74984711e379b56471.jpg" width="500">
</center>

## 2. Logic Circuits

When you connect logic gates together, you create a logic circuit. These fall into two distinct categories based on how they handle time and memory.

### Combinational Logic Circuits

* **Definition:** The output depends *only* on the current inputs at that exact moment.
* **Memory:** None. They have no concept of "past" or "state".
* **Examples:** Adders (ALU), Decoders, Multiplexers (MUX).
* **Analogy:** A padlock. It opens the moment the correct key is inserted, regardless of what key was inserted 5 minutes ago.

<center>
    <img src="https://image2url.com/r2/default/images/1768474967874-60a52419-e857-4675-98eb-f27310b1ca5f.png" width="600">
</center>

### Sequential Logic Circuits

* **Definition:** The output depends on the current inputs **`AND`** the past history (current state) of the circuit.
* **Memory:** Yes. They use feedback loops or memory elements (Flip-Flops, Latches) to store bits.
* **Examples:** Registers (like the MIPS registers `$t0`, `$a0`), Counters, RAM.
* **Analogy:** A combination lock. You must turn the dial left, *then* right, *then* left. The lock "remembers" the previous positions.

## 3. Combinational Logic Rules

These rules ensure that your digital circuit works reliably electrically and doesn't destroy itself.

### A. Never Connect Two Outputs Together

* **The Rule:** You must never wire the output of one gate directly to the output of another gate.
* **The Reason (Short Circuit):** If Gate A outputs a **1** (5V) and Gate B outputs a **0** (0V), and you connect them, you create a direct path from power to ground with almost zero resistance.
* **The Consequence:** This is a "short circuit." Huge current will flow, the wires will heat up, and you will likely burn out the transistors in the gates permanently.

### B. Never Connect an Output Directly to VCC or Ground

* **The Rule:** A logic gate's output pin should only connect to other *inputs* (or to a resistor/LED), but never directly to the positive voltage rail (VCC) or the Ground rail (GND).
* **The Reason (Fighting the Power Supply):** This is essentially a specific version of Rule #1. The power supply (VCC) is a giant "output" that is always **1**. If your gate tries to output a **0**, it will fight the power supply. The power supply always wins, and your gate will be destroyed.
* **The Consequence:** Immediate physical damage to the chip.

### C. Never Leave an Input "Floating" (Blank)

* **The Rule:** Every input pin on every logic gate must be connected to *something* (either another output, VCC, or Ground).
* **The Reason (Antenna Effect):** In modern CMOS chips (the technology used for almost all processors), the inputs have very high impedance (resistance). If you leave a wire disconnected, it acts like a little radio antenna. It picks up random electromagnetic noise from the air (static, radio waves, mains hum).
* **The Consequence:** The gate will randomly flip between 0 and 1, causing your circuit to behave unpredictably. This can also cause the transistor to hover in a "half-on" state, causing it to overheat.

## 4. Logic Function Representation

There are 3 different ways to represent a logic function:

### Logic Networks Diagram

The logic diagram is the graphical representation of the combinational logic circuit. It consists of logic gates wired together in a specific manner to perform the desired function. The logic diagram details each and every logic gate used in the combinational logic by using a specific graphical symbol of each gate.

### Truth Table

The Truth Table represents the logical function of a combinational logic circuit in tabular form which lists all of the possible combinations of inputs and their states against the respective output(s). Simply, it lists the logical output(s) for each of the possible combinations of its inputs.

### Logic Expression

The Boolean Algebra expresses the combinational logic in the form of an expression or equation. The expression relates the input and output variables to produce the desired function or output. The expression can be evaluated by feeding the input states i.e. “0”s and “1”s and checking for the outcome against the input states.

<center>
    <img src="https://image2url.com/r2/default/images/1768474700401-930f2837-8531-4a3a-ad12-96580d858d77.png" width="600">
</center>

## 5. Logic Expression Minimization

We just investigate combinational logic circuits that are created from only `NAND` gates since any logic function can by synthesized by using only `NAND` gates (or `NOR` gates). In chip manufacturing (CPU, RAM), producing billions of identical transistors (all `NAND`) is cheaper, more stable, and easier to design than manufacturing a mix of different gate types. 

* Many equivalent logic functions exist for a particular truth table.
* The cost of a logic function can be measured in terms of gate count.
* The objective of minimization is to reduce the implementation cost.
* An expression is minimal if there is no other equivalent expression of lower cost.
* Minimization is performed as a series of algebraic manipulations.
* Most important rule used in minimization is distributive rule.

To minimize the implementation cost, we first need to simplify the logic expression. There are 2 ways to do it:

### Sum of Products

Logic expressions are often shown in sum of products form, e.g., $f=\overline{x_1}x_2 + x_1\overline{x_2}$. Operations are performed in the following order: `NOT`, `AND`, `OR`. Sum of products form makes it easier to derive the logic network since:

* For each product term, connect input variables to `AND` gate, use `NOT` gates wherever input needs to be inverted.
* Connect `AND` gate outputs to `OR` gate.

Sum-of-products form can be derived from truth table by the following steps:

1. Identify rows in the table for which $Q = 1$.
2. For each of such rows, write down a product term (`AND`) which includes all the input variables. `NOT` operator is applied to variables whose values are $0$ in that row. 
3. Add the `+` operators (`OR`) between the product terms.

<center>
    <img src="https://image2url.com/r2/default/images/1768477478637-e77aef3c-95ff-48f3-bde2-5c182ed591ed.png" width="500">
</center>

### Karnaugh Map (K-map)


