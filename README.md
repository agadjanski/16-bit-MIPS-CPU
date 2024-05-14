# 16 bit CPU in Logisim
This is a 16-bit processor. In particular, all the registers are 16-bit, and all the instructions are also 16-bit.
The processor has 8 general-purpose registers (`$r0 - $r7`).
The data and instructions are stored in two separated memories, each having its own address space. Both the data and the instruction memory are addressed by 16 bits.

The processor supports the following instructions:
+ `li $r1, x` load immediate number to one register
+ `add $r1, $r2, $r3` add two register numbers (`$r2` and `$r3`) and store the result in one register (`$r1`)
+ `and $r1, $r2, $r3` bit-wise logical `AND` of two register numbers (`$r2` and `$r3`), and store the result in a register (`$r1`)
+ `or $r1, $r2, $r3`  bit-wise logical `OR` of two register numbers (`$r2` and `$r3`), and store the result in a register (`$r1`)
+ `neg $r1, $r2`      negate a register number (`$r2`) and store the result in the register (`$r1`)
+ `load $r1, $r2`     load a 16-bit number from data memory to a register (`$r1`). The data memory address is the value in a register (`$r2`)
+ `store $r1, $r2`    load a 16-bit number from a register (`$r1`) to data memory. The data memory address is the value in a register (`$r2`)
+ `move $r1, $r2`     copy the value of one register (`$r2`) to another register (`$r1`)
+ `addi $r1, $r2, x`  add a register number (`$r2`) and an immediate number (`x`), and store the result in one register (`$r1`)
+ `andi $r1, $r2, x ` bit-wise logical `AND` of a register number (`$r2`) and an immediate number `x`, and store the result in a register (`$r1`)
+ `ori $r1, $r2, x `  bit-wise logical `OR` of a register number (`$r2`) and an immediate number `x`, and store the result in a register (`$r1`)
+ `ble $r1, $r2, x `  if the first register number (`$r1`) is smaller than or equal to the second register number (`$r2`), then jump to address (`PC + x`)
+ `slt $r1, $r2, $r3 ` if the register number (`$r2`) is less than the register number (`$r3`), then `$r1=1`; otherwise `$r1=0`
+ `lsl $r1, $r2, $r3` logical-shift a register number (`$r2`) to left for several digits (`$r3`), and store the result in a register (`$r1`)
+ `lsr $r1, $r2, $r3` logical-shift a register number (`$r2`) to right for several digits (`$r3`), and store the result in a register (`$r1`). The shift-in high bits are all 0
+ `jump x`            unconditional branch to address `PC + x`
+ `call x`            jump to address (`PC + x`), where `x` is an immediate number, and at the same time save the address of the next instruction to a special register `$ra` (`$ra` is not one of the 8 general registers `$r0 - $r7`)
+ `rtn`               jump to address stored in $ra
+ `reboot`            directly jump to address 0
+ `halt`              all registers (including `PC`, the 8 general purpose registers and all other registers) in the processor are disabled (so the processor halts)

## Pipelining
This processor uses five stage pipelining, and handles the control hazards and data hazards properly. It can stall IF and ID phases and flush the ID/EX circuit so nothing that has been previously sent is executed while it is waiting.
It is worth mentioning that the flush resets all the registers to zero which is equivalent to the instruction `or $r1, $r1, $r1`, which at first seems like it changes the value of the first register, but in reality, the bitwise expression `x = x or x` does not change anything (this is why the opcode and the funct for or are `0000` and `000` respectively), the Arguments circuit takes that into account
The hazard unit also checks whether the instruction that is currently in the EX phase is any J-type instruction or a successful branch. If so, the IF/ID is being flushed (so no instructions that are fetched after the jump/branch can be executed).

