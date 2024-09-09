
**<u>Definitions:</u>**

**Heap:** Used for dynamic memory (can change during runtime) during program execution to allocate & eliminate values that the program no longer needs.  

**Stack:** Used for local variables & function parameters to help control the program flow. Last in first out (LIFO). The stack grows down.  

**Mnemonic:** Identifies the instruction to execute 

**Operand:** Identifies information for the instruction, such as: registers or data.  

	  • Immediate Operands: Fixed values 
	
	 • Register Operands: Refer to registers, like ecx.  
	
	 • Memory Address Operands: Refer to a memory address with a value of interest. Denoted by a value, register, or equation between brackets, like [ecx].  
	
	** x86 uses little endian & networks use big endian, be careful not to reverse important  data on accident during analysis. ** 


**x86 -32 Bit Registers:** In 32 bit CPUs, the registers begin with "E", in 64 bit ones they begin with "R".

| **General Registers** | **Segment Registers** | **Status Flag** | **Instruction Pointer** |
| --------------------- | --------------------- | --------------- | ----------------------- |
| EAX (AX, AH, AL)      | CS                    | EFLAGS          | EIP                     |
| EBX (BX, BH, BL)      | SS                    |                 |                         |
| ECX (CX, CH, CL)      | DS                    |                 |                         |
| EDX (DX, DH, DL)      | ES                    |                 |                         |
| EBP (BP)              | FS                    |                 |                         |
| ESP (SP)              | GS                    |                 |                         |
| ESI (SI)                      |                       |                 |                         |


| Register                   | Purpose                                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------------------- |
| EAX (Accumulator Register) | Arithmetic operations & function's return value.                                                  |
| ECX (Counter Register)     | Used in shift/rotate instructions & loops. Also passes parameters in fastcall conventions.        |
| EDX (Data Registers)       | Arithmetic operations & input/output operations. Also passes parameters for fastcall conventions. |
| EBX (Base Register)        | Pointer to data                                                                                   |
| ESI (Source Index)         | Points to a source in string operations                                                           |
| EDI (Destination Index)    | Points to a destination in stream operations.                                                     |
| ESP (Stack Pointer)        | Points to the top of the stack                                                                    |
| EBP (Base Pointer)         | Points to the base of the stack.                                                                  |


**General Registers are 32 bits and can be referenced as 32 or 16 bits (edx refers to all 32 bits, dx is used to reference the lower 16 bits).**

**EAX, EBX, ECX, and EDX can also be referenced as 8 bit values using a the lowest 8 bits or the second set of 8 bits (AL refers to the lowest 8 bits of EAX, AH references the second set of 8 bits). **

	• General Registers: Used by the CPU during execution. 
	
	• Segment Registers: Used to track sections of memory. 
	
	• Status Flags: Used to make decisions.  
	
	• Instruction Pointers: Used to keep track of the next instruction to execute.    


**Flags:** 

<u>Zero Flag (ZF)</u>: Will be set to 1 if the result is zero, otherwise set to 0. 

<u>Carry Flag (CF)</u>: Will be set to 1 if an operation results in a carry/borrow. 

<u>Sign Flag (SF)</u>: 0 indicates a positive value, 1 indicates a negative value. 

<u>Parity Flag (PF)</u>:  Will be set to 1 if the least-significant byte of the result has an even number of 1 bits. Otherwise it will be cleared. 

<u>Overflow Flag (OF)</u>: Set if the value is too large to fit in the destination operand. 


**EIP (Instruction Pointer):** Contains the memory address of the next instruction to be executed for a program. Sole purpose is to tell the CPU what to do next. 

*When EIP is corrupted (that is, it points to a memory address that does not contain legitimate program code), the CPU will not be able to fetch legitimate code to execute, so the program running at the time will likely crash. When you control EIP, you can control what is executed by the CPU, which is why attackers attempt to gain control of EIP through exploitation. Generally, attackers must have attack code in memory and then change EIP to point to that code to exploit a system.*

**Stack:** 

ESP will always point to the top of the stack. If a value is pushed (added) to the stack, ESP will be decremented. If a value is popped from the stack (read), ESP will be incremented. 

Since the memory unit's size is 4 bytes, the decrement/increment will take place in jumps of 4 

**Function Prologue:** Creates the stack frame.

	push ebp (adds the original EBP value to the stack so it can be restored at the epilogue.)
	
	mov ebp, esp (moves the top of the stack pointer to EBP so that the base pointer will point to the top of the stack. )

**Function Epilogue:** Clears the stack, reverses the prologue, and returns to the calling function. 

	mov esp, ebp (Assigns the value in EBP to ESP, so that ESP will now point to the base of the function.)
	
	pop ebp (Pops the original stack base pointer back to the EBP so that the base of the stack is restored to its value bfore the prologue.)

	ret (Jumps to the next instruction in the calling instruction.)


**Instructions:**

**mov**: The most common instruction. Moves data into the registers & memory. Format for x86 is 

		  mov destination, source

**lea:** Stands for load effective instruction. Puts a memory address into the destination. Also used for calculating values because it uses fewer instructions







