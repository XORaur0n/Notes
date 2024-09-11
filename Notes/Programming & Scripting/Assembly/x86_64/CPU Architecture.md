
**Control Unit:** Moves & Controls Data

**Arithmetic/Logic Unit (ALU):** Performs various mathematical & logical operations as requested from a program via Assembly

The way in which and how efficiently a CPU processes instructions depends on its Instruction Set Architecture (ISA). There are various ISAs but two important ones are 

	RISC: 
	
		Handles more simple instructions, taking more CPU cycles, but uses less power & has shorter cycles

	CISC: 
	
	Handles fewer, more complex instructions, which take fewer cycles but have longer cycles and use more power

-----------------------------------------

**Instruction Cycle:** The cycle it takes the CPU to process a single instruction. 

| Instruction | Description                                                                                                                                 |
|-------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| 1. Fetch	   | Takes the next instruction's address from the Instruction Address Register (IAR), which tells it where the next instruction is located.<br> |
| 2. Decode	  | Takes the instruction from the IAR, and decodes it from binary to see what is required to be executed.<br>                                  |
| 3. Execute	 | Fetch instruction operands from register/memory, and process the instruction in the ALU or CU.<br>                                          |
| 4. Store	   | Store the new value in the destination operand.<br>                                                                                         |

	All of the stages in the instruction cycle are carried out by the Control Unit, except when arithmetic instructions need to be executed "add, sub, ..etc", which are executed by the ALU.

Each instruction cycle, takes multiple CPU clock cycles to complete (depending on the processor architecture & the instruction's complexity). 

Because modern processors are multi-core & multi-thread, they can execute multiple instructions/clock cycles in tandem.



