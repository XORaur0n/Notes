
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
|                   |                                                                                                                  |

**Pointer Registers:** Used to store important address pointers

| Register | Description                                                              |
| -------- | ------------------------------------------------------------------------ |
| rbp      | Base Stack Pointer: points to the beginning of the stack                 |
| rsp      | Current Stack Pointer, points to the current location (top) of the stack |
| rip      | Instruction Pointer, holds the address of the next instruction           |

