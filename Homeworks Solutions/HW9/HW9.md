### **Problem 1: Direct Input to Output**
**Goal:** Write a program to read the switch block and show the exact value directly on the LED bank.

**Detailed Explanation:**
* **Hardware Setup:** The program starts by loading the base address for the hardware devices, which is `0x80000000`, into register `$8`.
* **The Loop:** It uses a continuous loop called `poll_loop`. 
* **Reading:** Inside the loop, it uses the `lw` (load word) command at offset `16` to copy the state of the physical switches into register `$9`.
* **Writing:** It immediately uses the `sw` (store word) command to push that exact value to offset `12`, which is wired to the LED bank. This creates a real-time hardware mirror.

---

### **Problem 2: Toggle LED with a Button**
**Goal:** Read the state of button B0. When pressed, it toggles the on/off state of LED L0.

**Detailed Explanation:**
* **State Tracking:** Register `$12` is set up to act as our "memory" for the LED state, starting at 0 (off).
* **Waiting for a Press:** The `wait_press` loop reads the button hardware. It uses the `andi` command with the mask `0x0400` to ignore everything except button B0. It stays in this loop until the button is pushed.
* **The Toggle:** When pressed, it uses `xori $12, $12, 1` to flip the lowest bit from 0 to 1, or 1 to 0. This new state is sent to the LEDs.
* **Waiting for a Release:** If we do not wait for the user to let go, the computer will loop thousands of times a second and the LED will flicker wildly. The `wait_release` loop pauses the program until B0 goes back to 0.

---

### **Problem 3: Multiple Button Commands**
**Goal:** Use three buttons for different commands. B0 shows the raw switch value, B1 shows the value shifted left, and B2 shows the inverted bit pattern.

**Detailed Explanation:**
* **The Master Loop:** The `poll_inputs` loop checks all three buttons. It uses different masks to spot which button was pushed: `0x0400` for B0, `0x0800` for B1, and `0x1000` for B2.
* **B0 (Raw Input):** It simply saves the switch value directly to the LED address.
* **B1 (Shift Left):** It takes the switch value and uses the `sll` (shift left logical) command to move all the bits one space to the left, which multiplies the value by 2 in binary.
* **B2 (Invert):** It uses `xori` with the mask `0x03FF` (which covers all 10 switches) to flip the bits. A switch that is ON becomes OFF, and an OFF switch becomes ON.
* **Universal Release:** The `wait_release` phase uses the mask `0x1C00` to combine all three button checks, making sure the user's hand is completely off the board before going back to the master loop.

---

### **Problem 4: Counter Stored in RAM**
**Goal:** Create a counter saved in the RAM memory. Button B0 adds one, B1 subtracts one, and B2 resets it to zero. Show the binary value on the LEDs.

**Detailed Explanation:**
* **Memory Usage:** Unlike the previous programs that kept numbers in the CPU, this program saves the counter out to the main data memory (RAM) at address `0x10010000`. 
* **Updating the Number:** When a button is pressed, the program must first `lw` (load) the number from RAM into a CPU register. It does the math (`addiu` for +1 or -1), and then uses `sw` (store) to push the new total back into the RAM.
* **Resetting:** For button B2, the program bypasses loading entirely and simply stores a `0` straight into the RAM address. 
* **Display:** After any change, the program loads the fresh number from RAM and sends it to the LED address.

---

### **Problem 5: 7-Segment Display Counter**
**Goal:** Add a two-digit screen. Button B1 adds one, B0 subtracts one, and B2 resets it to 0.

**Detailed Explanation:**
* **Data Mapping:** A 7-segment display requires specific binary patterns to draw numbers. The program uses a data array (`seg_map`) holding 10 patterns representing the digits 0 through 9.
* **Digit Splitting:** The hardware only takes one input word to control both digits. To handle this, the program takes the main counter (for example, 45) and repeatedly subtracts 10 to separate it into a "tens" digit (4) and a "units" digit (5).
* **Combining for Output:** The program looks up the drawing pattern for '4' and the pattern for '5'. It shifts the 'tens' pattern left by 8 bits (`sll`) so it lines up with the correct pins for the second digit on the hardware, and combines them into a single signal to send to the screen.
* **Limits:** Logic is added so the counter stops at 0 (so it does not draw broken characters for negative numbers) and rolls over from 99 back to 0.

---

### **Problem 6: 8x8 LED Matrix Symbols**
**Goal:** Draw symbols (left, right, up, down arrows) on an 8x8 LED matrix. Pressing B1 cycles through the pictures.

**Detailed Explanation:**
* **Image Storage:** An 8x8 matrix has 64 tiny lights. Since a standard MIPS register only holds 32 bits, one full picture requires two words of data (a top half and a bottom half). The four arrow pictures are hardcoded in the `.data` section.
* **State Machine:** The program uses a register to track the current "state" from 0 to 3. 
* **Memory Navigation:** To find the right picture, the program multiplies the state number by 8 bytes (since each picture takes up 8 bytes of space). It adds this offset to the base memory address to find the exact location of the current image.
* **Drawing:** It loads the top word and bottom word from memory and sends them to the two separate hardware addresses required to control the full matrix block. Pressing B1 adds 1 to the state and forces it to loop back to 0 after reaching 3 using a modulo operation (`andi $12, $12, 3`).