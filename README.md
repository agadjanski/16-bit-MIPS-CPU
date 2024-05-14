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
+ 
