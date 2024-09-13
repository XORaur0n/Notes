
Refers to the syntax & semantics of the Assembly language for each architecture. 

| Component         | Description                                                                                                               | Example                                |
|-------------------|---------------------------------------------------------------------------------------------------------------------------|----------------------------------------|
| Instructions	     | The instruction to be processed in the opcode operand_list format. There are usually 1,2, or 3 comma-separated operands.	 | add rax, 1, mov rsp, rax, push rax<br> |
| Registers         | Used to store operands, addresses, or instructions temporarily.	                                                          | rax, rsp, rip<br>                      |
| Memory Addresses	 | The address in which data or instructions are stored. May point to memory or registers.	                                  | 0xffffffffaa8a25ff, 0x44d0, $rax<br>   |
| Data Types	       | The type of stored data.	                                                                                                 | byte, word, double word<br>            |


**Complex Instruction Set Computer (CISC):** 

Used mainly by Intel & AMD chips

Relies on being able to execute more complex instructions at once to reduce the overall instructions being run. 

CISC chips usually consume more power, have less  instructions (shortening the assembly code), and relies on hardware optimization instead of software. 

