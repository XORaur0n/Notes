
**Data Movement:** 

| Instruction | Description                                  | Example                                    |
| ----------- | -------------------------------------------- | ------------------------------------------ |
| `mov`       | Move data or load immediate data             | `mov rax, 1` → `rax = 1`                   |
| `lea`       | Load an address pointing to the value        | `lea rax, [rsp+5]` → `rax = rsp + 5`       |
| `xchg`      | Swap data between two registers or addresses | `xchg rax, rbx` → `rax = rbx`, `rbx = rax` |

	mov doesn't change the source operand, so it operates more like a copy function

-------------------------------------------

**Unary Instructions: (1 operand)** 

| Instruction | Description    | Example                            |
| ----------- | -------------- | ---------------------------------- |
| `inc`       | Increment by 1 | `inc rax` -> `rax++` or `rax += 1` |
| `dec`       | Decrement by 1 | `dec rax` -> `rax--` or `rax -= 1` |

-------------------------------------------

**Binary Instructions: (2 operands)** 

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

