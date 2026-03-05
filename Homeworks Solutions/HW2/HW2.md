# ECE341 HOMEWORK 2

## Problem 4

### Step 1: State transition table

| Present State ($Q_2Q_1Q_0$) | Next State / Input D ($D_2D_1D_0$) | Note |
| --- | --- | --- |
| **0 0 0** | **0 1 0** | 0 → 2 |
| 0 0 1 | X X X | X |
| **0 1 0** | **0 1 1** | 2 → 3 |
| **0 1 1** | **1 1 1** | 3 → 7 |
| 1 0 0 | X X X | X |
| 1 0 1 | X X X | X |
| 1 1 0 | X X X | X |
| **1 1 1** | **0 0 0** | 7 → 0 |

### Step 2: Karnaugh map

| Q2Q1 \ Q0 | 0 | 1 |
|-----------|---|---|
| 00        | 0 | X |
| 01        | 1 | 1 |
| 11        | X | 0 |
| 10        | X | X |

### Step 3: Logic expressions and circuit 

$D_2=Q_0\cdot \overline{Q_2}$

$D_1=\overline{Q_2}$

$D_0=Q_1\cdot \overline{Q_2}$

## Problem 5

### Step 1: State transition table

| Current State () | Input C | Next State () | Flip-Flop Inputs () |
| --- | --- | --- | --- |
| **0 0 0** | **1** | **0 1 0** |  |
| **0 0 0** | **0** | **1 1 1** |  |
| **0 1 0** | **1** | **0 1 1** |  |
| **0 1 0** | **0** | **0 0 0** |  |
| **0 1 1** | **1** | **1 1 1** |  |
| **0 1 1** | **0** | **0 1 0** |  |
| **1 1 1** | **1** | **0 0 0** |  |
| **1 1 1** | **0** | **0 1 1** |  |
| Other states | X | X X X | X X, \quad X X, \quad X X |

### Karnaugh map
