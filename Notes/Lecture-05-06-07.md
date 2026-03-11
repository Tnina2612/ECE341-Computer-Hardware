# **Computer Arithmetic: Addition and Multiplication**

## **1. Number Representation: 2's Complement**

Two's complement is the most common integer number system used in computers because it simplifies the hardware required for addition and subtraction.

### **Key Concepts**

* **Range:** An $N$-bit binary digit can represent any integer between $-2^{N-1}$ and $+2^{N-1}-1$.
* **Sign Bit:** The Most Significant Bit (MSB) indicates the sign of the integer: 0 for positive and 1 for negative.
* **Obtaining Representation:** To find the 2's complement of a signed integer, we invert all bits (this is the 1's complement) and then add 1 to the result.

### **Arithmetic Visualization**

Arithmetic can be viewed on a "number circle":

* **Addition:** Moving **clockwise** adds positive numbers.
* **Subtraction:** Moving **counter-clockwise** adds negative numbers.

<center>
    <img src="https://image2url.com/r2/default/images/1773224863566-8171055f-766f-403b-b415-9d6f7bd28a07.png" width="400">
</center>

## **2. Addition and Subtraction Logic**

### **Binary Addition**

* Bit pairs are added from the **Least Significant Bit (LSB)** to the **MSB**.
* Each stage takes two bit values and a **carry-in** as input, generating a **sum** and a **carry-out**.
* The carry-out of one bit position becomes the carry-in for the next.

### **Binary Subtraction**

Subtraction is performed by adding the 2's complement of the second operand to the first operand ($X - Y = X + \text{2's complement of } Y$). In hardware, this is equivalent to:

$$X - Y = X + \overline{Y} + 1$$

This is implemented by inverting the bits of $Y$ and setting the initial carry-in ($c_0$) to **1**.

## **3. Overflow in Integer Arithmetic**

Overflow occurs when the result of an addition or subtraction cannot be represented by the same number of bits as the operands.

### **Detection Conditions**

* **For Addition:**
  * Overflow **cannot** occur if the operands have different signs.
  * Overflow **occurs** if the two operands have the **same sign** but the result has a **different sign**.

* **General Rule (Addition & Subtraction):**
  * Overflow occurs when the **carry-in to the MSB** is different from the **carry-out of the MSB**.
  * This rule can be explained by looking at the below truth table.

### **Overflow Logic Circuits**

1. **Based on signs:** $\text{Overflow} = x_{n-1}y_{n-1}\overline{s}_{n-1} + \overline{x}_{n-1}\overline{y}_{n-1}s_{n-1}$.
2. **Based on carries (simpler):** $\text{Overflow} = c_n \oplus c_{n-1}$.

<center>
    <img src="https://image2url.com/r2/default/images/1773223885213-51a16fac-3765-43b8-bf4e-3018b377ef64.png" width="500">
</center>

## **4. Hardware Design: Ripple-Carry Adders**

### **The Full Adder (FA)**

A single stage of addition is performed by a Full Adder with the following logic:

* **Sum:** $s_i = x_i \oplus y_i \oplus c_i$
* **Carry-out:** $c_{i+1} = y_i c_i + x_i c_i + x_i y_i$

<center>
    <img src="https://image2url.com/r2/default/images/1773224997289-7748d0f1-aa3c-4ab0-83bc-bd23350b537e.png" width="500">
</center>

### **The Ripple-Carry Adder**

An $n$-bit adder is formed by cascading $n$ Full Adder blocks with $c_n$ connects to carry flag. The carry "ripples" through the stages from LSB to MSB.

* **Critical Path:** The carry propagation is the slowest part of the circuit (the critical path) because each stage must wait for the carry from the previous stage.
* **Add Time (Gate Delays):**
  * $s_0$ is available after 1 gate delay.
  * $c_1$ is available after 2 gate delays.
  * For an $n$-bit ripple adder, the final sum ($s_{n-1}$) is available after **$2n-1$** gate delays and the final carry ($c_n$) after **$2n$** delays.

For subtractor, we set carry-in $c_0$ into the LSB position to be $1$ and invert $y_i$'s to perform subtraction. Adder and subtractor can be combined into a single block with $c_0$ is considered as the control bit.

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/62a4d7ff-0168-4ea0-9749-4be03baa3830.png" width="350">
</center>

## **5. Fast Addition: Carry Lookahead Adders (CLA)**

To avoid the ripple delay, CLA computes all carries in parallel.

### **Generate and Propagate Functions**

* **Generate:** $G_i = x_iy_i$. Stage $i$ generates a carry-out independent of carry-in.
* **Propagate:** $P_i = x_i + y_i$ (often implemented as $x_i \oplus y_i$). Stage $i$ propagates a carry-in to its output.
* $G_i$ and $P_i$ depend only on $x_i$ and $y_i$, meaning they can be computed in **1 gate delay**.

### **CLA Logic**

Carries are calculated using the expanded formula:

$$c_{i+1} = G_i + P_i G_{i-1} + P_i P_{i-1} G_{i-2} + \dots + P_i P_{i-1} \dots P_0 c_0$$

* **Theoretical Delay:** Any $c_{i+1}$ can be computed in 3 gate delays ($1$ for $G/P$, $1$ for AND terms, $1$ for ORing).
* **Sum Delay:** The total delay for the sum output is **4 gate delays**, independent of $n$ since it requires one additional XOR delay after carries are computed.

### **Practical Limitations (Fan-in)**

In practice, the number of inputs to gates is limited.

  * For a 4-bit CLA, the MSB carry-out ($c_4$) requires a fan-in of **5**, which is the common practical limit.
  * To add larger operands, **Blocked Carry-Lookahead adders** must be used.

<center>
    <img src="https://image2url.com/r2/default/images/1773227497386-9df0ea43-b022-4806-8596-271d4c3989fe.png" width=500">
</center>

## **6. Blocked Carry-Lookahead Adders**

### **Cascaded Blocks**

Multiple 4-bit CLA blocks can be cascaded. In a 32-bit blocked CLA (eight 4-bit blocks), carries ripple between blocks.

* $c_4$ is available after **3** delays.
* $c_8$ is available after **5** delays.
* $c_{32}$ is available after **17** delays.

### **Parallel Blocks (First-Level Functions)**

To further speed up the process, we can generate carries between blocks in parallel by calculating **first-level generate ($G_k^1$)** and **propagate ($P_k^1$)** functions for each block.

* A 16-bit adder using this method can produce sum outputs in **8 gate delays**, compared to 10 delays for simple cascading.

## **7. Multiplication of Unsigned Numbers**

Unsigned multiplication is essentially the addition of shifted versions of the multiplicand ($M$) based on the bits of the multiplier ($Q$).

### **Combinatorial Array Multiplier**

* Uses an array of $n$ $n$-bit adders to displace and add the multiplicand.
* **Inefficient:** The number of gates is proportional to **$n^2$**, making it impractical for 32 or 64-bit numbers.

### **Sequential Multiplier**

A more hardware-efficient solution that uses **one** $n$-bit adder and registers to accumulate partial products.

**Algorithm Steps:** 

1. **Initialization:** Load $M$ (multiplicand) and $Q$ (multiplier); set $C$ and $A$ (accumulator) registers to zero.
2. **Cycle (Repeated $n$ times):** 
   * If the LSB of register Q is 1, add $M$ to $A$ ($A = A + M$).
   * Shift right the combined $C, A, Q$ registers by one bit.
3. **Result:** After $n$ steps, registers $A$ and $Q$ together hold the final $2n$-bit product. The high-order half of the product is in $A$ and the low-order half is in $Q$.

> **Key Observation:** Adding a left-shifted multiplicand to an unshifted partial product is mathematically equivalent to adding an unshifted multiplicand to a right-shifted partial product.

<center>
    <img src="https://image2url.com/r2/default/images/1773228759716-8d2ff200-ab80-4027-8ba5-ede6ff95f2f0.png" width="500">
</center>

## **8. Signed Multiplication**

### **Sign Extension**

* When multiplying signed 2's-complement operands (e.g., $-13 \times +11$), the sign bit of the multiplicand must be extended to the left as far as the product extends.
* Failure to perform **sign extension** will result in an incorrect product.

### **Handling Negative Multipliers**

* **Positive Multiplier:** Unsigned hardware works if augmented with sign extension.
* **Negative Multiplier:** One method is to take the 2's complement of both operands (which doesn't change the product's value) and proceed as if the multiplier were positive.
* **Uniform Solution:** The **Booth algorithm** is preferred because it treats positive and negative operands uniformly.

<center>
    <img src="https://image2url.com/r2/default/images/1773246513445-ec681d0f-ad27-4398-aef0-c087639672eb.png" width="500">
</center>

## **9. The Booth Algorithm**

The Booth algorithm reduces the number of additions required by identifying blocks of 1s in the multiplier.

* **Logic:** It recognizes that a string of 1s (e.g., $0011110$) can be represented as a subtraction and an addition ($0100000 - 0000010$).
* **Operation Selection:**
    * When the multiplier scan moves from **0 to 1**, subtract the shifted multiplicand ($-1 \times M$).
    * When moving from **1 to 0**, add the shifted multiplicand ($+1 \times M$).
    * When the bits are the same (**0 to 0** or **1 to 1**), no addition occurs ($0 \times M$).

**Booth Multiplier Recoding Table** 

| Bit $i$ | Bit $i-1$ | Version of Multiplicand Selected |
| --- | --- | --- |
| 0 | 0 | $0 \times M$ |
| 0 | 1 | $+1 \times M$ |
| 1 | 0 | $-1 \times M$ |
| 1 | 1 | $0 \times M$ |

* **Efficiency:**
  * **Worst Case:** Alternating 0s and 1s result in $n$ summands.
  * **Best Case:** Long strings of 1s allow the algorithm to skip many additions.

<center>
    <img src="https://image2url.com/r2/default/images/1773246795699-b23a7951-4806-4a52-ac29-e413c18b6cc2.png" width="500">
</center>

## **10. Fast Multiplication: Bit-Pair Recoding**

Bit-pair recoding (also known as modified Booth recoding) further optimizes multiplication by halving the maximum number of summands.

* **Key Concept:** For $n$-bit operands, it guarantees a maximum of $n/2$ summands.

* **Implementation:** Multiplier bits are examined in overlapping groups of three ($i+1$, $i$, and the reference bit $i-1$) to select a version of the multiplicand.

**Bit-Pair Recoding Selection Table** 

| Bit $i+1$ | Bit $i$ | Bit $i-1$ | Multiplicand Selected |
| --- | --- | --- | --- |
| 0 | 0 | 0 | $0 \times M$ |
| 0 | 0 | 1 | $+1 \times M$ |
| 0 | 1 | 0 | $+1 \times M$ |
| 0 | 1 | 1 | $+2 \times M$ |
| 1 | 0 | 0 | $-2 \times M$ |
| 1 | 0 | 1 | $-1 \times M$ |
| 1 | 1 | 0 | $-1 \times M$ |
| 1 | 1 | 1 | $0 \times M$ |

<center>
    <img src="https://image2url.com/r2/default/images/1773246971008-e9faa019-55e8-48f3-9a6a-31a0fc56a168.png" width="500">
</center>
