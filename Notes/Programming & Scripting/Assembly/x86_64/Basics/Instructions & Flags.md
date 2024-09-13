
**Data Movement:** 

| Instruction | Description                                  | Example                                    |
| ----------- | -------------------------------------------- | ------------------------------------------ |
| `mov`       | Move data or load immediate data             | `mov rax, 1` → `rax = 1`                   |
| `lea`       | Load an address pointing to the value        | `lea rax, [rsp+5]` → `rax = rsp + 5`       |
| `xchg`      | Swap data between two registers or addresses | `xchg rax, rbx` → `rax = rbx`, `rbx = rax` |

	mov doesn't change the source operand, so it operates more like a copy function

-------------------------------------------

**Arithmetic Unary Instructions: (1 operand)** 

| Instruction | Description    | Example                            |
| ----------- | -------------- | ---------------------------------- |
| `inc`       | Increment by 1 | `inc rax` -> `rax++` or `rax += 1` |
| `dec`       | Decrement by 1 | `dec rax` -> `rax--` or `rax -= 1` |

-------------------------------------------

**Arithmetic Binary Instructions: (2 operands)** 

| Instruction | Description                                | Example                               |
|-------------|--------------------------------------------|---------------------------------------|
| `add`       | Add both operands                         | `add rax, rbx` -> `rax = rax + rbx`   |
| `sub`       | Subtract Source from Destination           | `sub rax, rbx` -> `rax = rax - rbx`   |
| `imul`      | Multiply both operands                     | `imul rax, rbx` -> `rax = rax * rbx`  |

-------------------------------------------

**Bitwise Instructions:** 

| Instruction | Description                                                  | Example                                       |
|-------------|--------------------------------------------------------------|-----------------------------------------------|
| `not`       | Bitwise NOT (invert all bits, 0->1 and 1->0)               | `not rax` -> `NOT 00000001` -> `11111110`     |
| `and`       | Bitwise AND (if both bits are 1 -> 1, if bits are different -> 0) | `and rax, rbx` -> `00000001 AND 00000010` -> `00000000` |
| `or`        | Bitwise OR (if either bit is 1 -> 1, if both are 0 -> 0)   | `or rax, rbx` -> `00000001 OR 00000010` -> `00000011` |
| `xor`       | Bitwise XOR (if bits are the same -> 0, if bits are different -> 1) | `xor rax, rbx` -> `00000001 XOR 00000010` -> `00000011` |

-------------------------------------------

**Loops:** 

| Instruction                 | Description                                                     | Example            |
| --------------------------- | --------------------------------------------------------------- | ------------------ |
| `mov rcx, [number of loops` | Sets the loop counter (`rcx`) to `x`                            | `mov rcx, 3`       |
| `loop`                      | Jumps back to the start of the loop until the counter reaches 0 | `loop exampleLoop` |

-------------------------------------------


**Unconditional Branching:** 

| Instruction | Description                                | Example       |
|-------------|--------------------------------------------|---------------|
| `jmp`       | Jumps to the specified label, address, or location | `jmp loop` |

-------------------------------------------

**Conditional Branching:** 

*D is destination*

| Instruction | Condition | Description                                        |
| ----------- | --------- | -------------------------------------------------- |
| `jz`        | D = 0     | Destination `equal to Zero`                        |
| `jnz`       | D != 0    | Destination `Not equal to Zero`                    |
| `js`        | D < 0     | Destination` is Negative`                          |
| `jns`       | D >= 0    | Destination `is Not Negative (i.e. 0 or positive)` |
| `jg`        | D > S     | Destination `Greater than Source`                  |
| `jge`       | D >= S    | Destination `Greater than or Equal to Source`      |
| `jl`        | D < S     | Destination `Less than Source`                     |
| `jle`       | D <= S    | Destination `Less than or Equal to Source`         |


RFLAGS Register:

RFLAGS as 64-bits just like the others, but doesn't hold values but flag bits instead. 

	Each bit turns to 1 or 0 depending on the previous isntruction

	Arithmetic operations set the RFLAG bits depending on their outcome. 


RFLAGS has a 32-bit sub-register (EFLAGS) & a 16-bit one (FLAGS). The ones we usually care about are: 

| Flag | Description                                      |
|------|--------------------------------------------------|
| CF   | Indicates whether there is a carry out from the most significant bit (used for unsigned arithmetic) |
| PF   | Indicates whether the number of set bits in the result is even (used for parity checking) |
| ZF   | Indicates whether the result of an operation is zero |
| SF   | Indicates whether the result of an operation is negative (based on the most significant bit) |

For reference here are all of the flags within the RFLAGS register: 


| Bit(s)      | 0          | 1        | 2           | 3        | 4                    | 5        | 6          | 7         | 8         | 9              | 10             | 11            | 12-13               | 14          | 15       | 16          | 17               | 18                               | 19                     | 20                        | 21                  | 22-63    |
| ----------- | ---------- | -------- | ----------- | -------- | -------------------- | -------- | ---------- | --------- | --------- | -------------- | -------------- | ------------- | ------------------- | ----------- | -------- | ----------- | ---------------- | -------------------------------- | ---------------------- | ------------------------- | ------------------- | -------- |
| Label       | CF (CY/NC) | 1        | PF (PE/PO)  | 0        | AF (AC/NA)           | 0        | ZF (ZR/NZ) | SF        | TF        | IF             | DF             | OF            | IOPL                | NT          | 0        | RF          | VM               | AC                               | VIF                    | VIP                       | ID                  | 0        |
| Description | Carry Flag | Reserved | Parity Flag | Reserved | Auxiliary Carry Flag | Reserved | Zero Flag  | Sign Flag | Trap Flag | Interrupt Flag | Direction Flag | Overflow Flag | I/O Privilege Level | Nested Task | Reserved | Resume Flag | Virtual-x86 Mode | Alignment Check / Access Control | Virtual Interrupt Flag | Virtual Interrupt Pending | Identification Flag | Reserved |


CMP: 

| Instruction | Description                                                                                 | Example                       |
| ----------- | ------------------------------------------------------------------------------------------- | ----------------------------- |
| `cmp`       | Sets RFLAGS by subtracting the second operand from the first operand (i.e., first - second) | `cmp rax, rbx` -> `rax - rbx` |

-------------------------------------------

