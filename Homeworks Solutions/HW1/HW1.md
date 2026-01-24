# ECE341 HOMEWORK 1

## Problem 1

Installation.

## Problem 2

Tutorial.

## Problem 3

* `NOT`: $\overline{a} = \overline{aa}$, using 1 `NAND` gate
* `AND`: $ab = \overline{\overline{ab}}$, using 2 `NAND` gates
* `OR`: $a + b = \overline{\overline{a+b}} = \overline{\overline{a}\cdot\overline{b}}$, using 3 `NAND` gates
* `XOR`: $a\overline{b} + \overline{a}b = \overline{\overline{a\overline{b} + a\overline{a} + \overline{a}b + b\overline{b}}} = \overline{\overline{a\overline{ab} + b\overline{ab}}} = \overline{\overline{a\overline{ab}}\cdot\overline{b\overline{ab}}}$, using 4 `NAND` gates
* `XNOR`: `NOT` `XOR`, using 5 `AND` gates

## Problem 4

* `NOT`: $\overline{a} = \overline{a + a}$, using 1 `NOR`
* `OR`: $a + b = \overline{\overline{a+b}}$, using 2 `NOR` gates
* `AND`: $ab = \overline{\overline{ab}} = \overline{\overline{a} + \overline{b}}$, using 3 `NOR` gates
* `XOR`: $a\overline{b} + \overline{a}b = \overline{\overline{a\overline{b} + a\overline{a} + \overline{a}b + b\overline{b}}} = \overline{\overline{\overline{a}(a+b)+\overline{b}(a+b)}} = \overline{\overline{\overline{a+\overline{a+b}} + \overline{b+\overline{a+b}}}}$, using 5 `NOR` gates since it is `NOT` of `XNOR`

## Problem 5

Since the internal structure of TTL 7400 IC is just the combination of 4 `NAND` gates, which are sufficient for a 2-input `XOR` gate, we just need to use 3 ICs to create a 4-input `XOR` gate.

## Problem 6

<center>
    <img src="https://i.ibb.co/bMH5dxvF/image.png" width="300">
</center>

Drawing K-map, we find that the logical expression of the circuit is $Y=X_1X_0 + X_2X_0 + \overline{X_2}\cdot\overline{X_1}\cdot\overline{X_0} = X_0(X_1+X_2) + \overline{X_0}\cdot\overline{X_1+X_2}$. That is clearly `XNOR` between $X_0$ and $X_1+X_2$, so we need to use 8 `NAND` gates.

## Problem 7

We just need to add `NOT` to 0-bit inputs, then `AND` all of inputs together with the unlock button.

## Problem 8

Note that when we `XOR` all of inputs, the output will be 1 if there are odd number of 1-bit inputs, 0 otherwise. Therefore, we need to `XOR` all 7 bits of input data and parity bit together. The parity bit depends on the last digit of your student ID. 