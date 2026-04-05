# ECE341 HOMEWORK 5

## Problem 1

### State Table

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/29d2328e-dac7-4adc-917c-0cc0989bc156.jpg" width="500">
</center>

### K-maps and Derived Expressions

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/a1b5a7e9-97fd-4df9-9fb7-29be7ba09908.jpg" width="500">
</center>

Since this is a Moore FSM, the output depends only on the current state. $Z$ is only high when the machine is in state $S3$ ($Q_1Q_0 = 11$), or $Z = Q_1 \cdot Q_0$.

## Problem 2

### State Table

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/18394bb6-b18e-4d11-85e5-4fe4b27e70c3.jpg" width="500">
</center>

### K-maps and Derived Expressions

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/3b505366-dde1-495a-b9fe-19646e41cbd7.jpg" width="500">
</center>

## Problem 3

### State Table

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/7bc49cef-f533-4369-bb16-b6782cbade3e.jpg" width="500">
</center>

* Detection for 1 (0001): $Is\_1 = \overline{K_3} \cdot \overline{K_2} \cdot \overline{K_1} \cdot K_0$ 
* Detection for 3 (0011): $Is\_3 = \overline{K_3} \cdot \overline{K_2} \cdot K_1 \cdot K_0$
* Detection for 4 (0100): $Is\_4 = \overline{K_3} \cdot K_2 \cdot \overline{K_1} \cdot \overline{K_0}$

### K-maps and Derived Expressions

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/7bc49cef-f533-4369-bb16-b6782cbade3e.jpg" width="500">
</center>

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/ae4099d3-6b57-4a2a-bf81-c4066dd41606.jpg" width="500">
</center>

## Problem 4

### State Table

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/f6364986-4c1c-4b92-83d5-1234c35f4d76.jpg" width="500">
</center>

* Detection for 0 (0000): $Is\_1 = \overline{K_3} \cdot \overline{K_2} \cdot \overline{K_1} \cdot \overline{K_0}$ 
* Detection for 2 (0010): $Is\_3 = \overline{K_3} \cdot \overline{K_2} \cdot K_1 \cdot \overline{K_0}$
* Detection for 5 (0101): $Is\_4 = \overline{K_3} \cdot K_2 \cdot \overline{K_1} \cdot K_0$

### K-maps and Derived Expressions

<center>
    <img src="https://pub-1407f82391df4ab1951418d04be76914.r2.dev/uploads/b294be60-9cd2-4e72-856c-99bdb8407bbe.jpg" width="500">
</center>
