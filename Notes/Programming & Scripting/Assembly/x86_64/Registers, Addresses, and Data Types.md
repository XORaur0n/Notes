
**Registers:** The fastest components within the computer but can only hold a few bytes at a time. 

-----------------------------------------

**Data Registers:** Used for storing instructions/syscall arguments. 

| Register          | Description                                                                                                      |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
| rax (Accumulator) | Frequently used for arithmetic operations and as a return value in functions                                     |
| rbx (Base)        | Often used as a base address for memory operations                                                               |
| rcx (Count)       | Commonly used as a counter for loops and shifts                                                                  |
| rdx (Data)        | Often used in arithmetic operations such as multiplication and division, and as an additional function parameter |
| r8 - r15          | General purpose registers, can be used when all others are in use                                                |

-----------------------------------------

**Pointer Registers:** Used to store important address pointers

| Register | Description                                                              |
| -------- | ------------------------------------------------------------------------ |
| rbp      | Base Stack Pointer: points to the beginning of the stack                 |
| rsp      | Current Stack Pointer, points to the current location (top) of the stack |
| rip      | Instruction Pointer, holds the address of the next instruction           |

-----------------------------------------

**Sub-Registers:** Every 64 bit register can be divided into smaller registers containing the lower bits like: 

1 byte (8-bits), 2 bytes (16-bits), and 4 bytes (32-bits)

They can be accessed as: 

| Size in bits | Size in bytes | Name                    | Example |
|--------------|---------------|-------------------------|---------|
| 16-bit       | 2 bytes        | the base name           | ax      |
| 8-bit        | 1 byte         | base name and/or ends with l | al      |
| 32-bit       | 4 bytes        | base name + starts with the e prefix | eax     |
| 64-bit       | 8 bytes        | base name + starts with the r prefix | rax     |

Below is a table of all essential 64-bit sub-registers: 


| Description                     | 64-bit Register | 32-bit Register | 16-bit Register | 8-bit Register |
| ------------------------------- | --------------- | --------------- | --------------- | -------------- |
| **Data/Arguments Registers**    |                 |                 |                 |                |
| Syscall Number/Return value     | rax             | eax             | ax              | al             |
| Callee Saved                    | rbx             | ebx             | bx              | bl             |
| 1st arg - Destination operand   | rdi             | edi             | di              | dil            |
| 2nd arg - Source operand        | rsi             | esi             | si              | sil            |
| 3rd arg                         | rdx             | edx             | dx              | dl             |
| 4th arg - Loop counter          | rcx             | ecx             | cx              | cl             |
| 5th arg                         | r8              | r8d             | r8w             | r8b            |
| 6th arg                         | r9              | r9d             | r9w             | r9b            |
| **Pointer Registers**           |                 |                 |                 |                |
| Base Stack Pointer              | rbp             | ebp             | bp              | bpl            |
| Current/Top Stack Pointer       | rsp             | esp             | sp              | spl            |
| Instruction Pointer 'call only' | rip             | eip             | ip              | ipl            |

-----------------------------------------

Memory Addresses: x86_64 chips obviously have 64-bit addresses, and their addresses range from 0x0 all the way to 0xffffffffffffffff. 