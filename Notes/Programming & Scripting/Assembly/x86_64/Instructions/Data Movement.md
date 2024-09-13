
| Instruction | Description                                  | Example                                    |
| ----------- | -------------------------------------------- | ------------------------------------------ |
| **mov**     | Move data or load immediate data             | `mov rax, 1` → `rax = 1`                   |
| **lea**     | Load an address pointing to the value        | `lea rax, [rsp+5]` → `rax = rsp + 5`       |
| **xchg**    | Swap data between two registers or addresses | `xchg rax, rbx` → `rax = rbx`, `rbx = rax` |

	mov doesn't change the source operand, so it operates more like a copy function

