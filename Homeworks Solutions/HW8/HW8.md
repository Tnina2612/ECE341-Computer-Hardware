# ECE341 HOMEWORK 6

### **Memory-Mapped I/O Base Addresses**

For all I/O operations, use $0\times80000000$ as the base address in register `$8`.

* **Keyboard Status**: `0($8)`.
* **Keyboard Data**: `4($8)`.
* **Console Output**: `8($8)`.
* **LED Control**: `0x000C` offset ($0\times8000000C$).

---

### **Problem 1: `printString(p)`**

Outputs a null-terminated string to the console.

* **Parameter**: `$4` = base address of the string.
* **Logic**: Loops until a null character ($0$) is found, writing each byte to the console address.

```mips
printString:
    lui $8, 0x8000          # Base I/O address
printLoop:
    lb $9, 0($4)            # Load character
    beq $9, $0, endPrint    # Stop if null
    nop
    sb $9, 8($8)            # Write to Console (0x80000008)
    addiu $4, $4, 1         # Next character
    j printLoop
    nop
endPrint:
    jr $31                  # Return
    nop
```

---

### **Problem 2: `readString(p)`**

Reads input from the keyboard, echoes characters to the console, and stops on a newline, carriage return, or space.

* **Parameter**: `$4` = memory address to store the string.
* **Stopping characters**: $0\times0A$ (LF), $0\times0D$ (CR), and $0\times20$ (Space).

Identical to `readString`, but skips the `sb $9, 8($8)` step so characters do not appear on the screen.

---

### **Problem 3: `atoi(p)`**

Converts an ASCII string to a signed integer.

* **Negative Handling**: Detects the `'-'` character (ASCII $45$) and sets a multiplier to $-1$.
* **Conversion Math**: Efficiently multiplies the current result by $10$ using bit-shifting:
  $result = (result \times 2^3) + (result \times 2^1) + digit$
* **Return**: The integer value is stored in `$2`.

```mips
atoi:
    addiu $2, $0, 0         # Initialize result
    addiu $8, $0, 1         # Sign flag (1 = positive)
    lb $9, 0($4)
    addiu $10, $0, 45       # Check for '-'
    bne $9, $10, atoi_loop
    nop
    addiu $8, $0, -1        # Set sign negative
    addiu $4, $4, 1
    lb $9, 0($4)
atoi_loop:
    addiu $10, $0, 48       # '0' ASCII
    slt $11, $9, $10
    bne $11, $0, atoi_end   # Exit if < '0'
    nop
    addiu $10, $0, 57       # '9' ASCII
    slt $11, $10, $9
    bne $11, $0, atoi_end   # Exit if > '9'
    nop
    sub $9, $9, $10         # Convert char to int
    sll $10, $2, 3          # res * 8
    sll $11, $2, 1          # res * 2
    add $2, $10, $11        # res * 10
    add $2, $2, $9          # Add digit
    addiu $4, $4, 1
    lb $9, 0($4)
    j atoi_loop
    nop
atoi_end:
    addiu $10, $0, -1
    bne $8, $10, atoi_ret
    nop
    sub $2, $0, $2          # Apply negative sign
atoi_ret:
    jr $31
```

---

### **Problem 4: `led(n)`**

Toggles the $n^{th}$ LED ($n$ is passed in `$4`).

* **State Persistence**: Uses a `.word` in memory (`led_state`) to remember which LEDs are currently on.
* **Bitwise Operation**: Uses `XOR` to flip the bit corresponding to the LED number.

### **Problem 5: `caesar(p, n)`**

Encrypts lowercase characters 'a' through 'z'.

* **Normalization**: If the shift value `n` is greater than $26$ or negative, it repeatedly adds/subtracts $26$ until $0 \leq n < 26$.
* **Wrapping**: Ensures that if a shift goes past 'z', it wraps back to 'a'.
