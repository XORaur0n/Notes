
Designed to be able to execute operations through the CPU without writing machine code manually each time. 

Think of `syscalls` as globally available `functions` provided by the OS. `Syscalls` take the arguments from the registers and execute the function with those arguments

To see all available `syscalls` on Linux: 

For 64-bit: 

	/usr/include/x86_64-linux-gnu/asm/unistd_64.h

For 32-bit: 

	/usr/include/x86_64-linux-gnu/asm/unistd_32.h

To see available `syscalls` on Windows: 

	Refer to Windows API documentation, use tools (Procmon, WinDbg, Ghidra/IDA, etc.) 


-------------------------------------------

**Calling Convention:** 


1. Save registers to stack

2. Set its `syscall` number in `rax`

3. Set its arguments in the registers

4. Use the `syscall` assembly instruction to call it

-------------------------------------------

**Arguments:** 

| Description                   | 64-bit Register | 8-bit Register |
| ----------------------------- | --------------- | -------------- |
| `Syscall` Number/Return value | `rax`           | `al`           |
| Callee Saved                  | `rbx`           | `bl`           |
| 1st arg                       | `rdi`           | `dil`          |
| 2nd arg                       | `rsi`           | `sil`          |
| 3rd arg                       | `rdx`           | `dl`           |
| 4th arg                       | `rcx`           | `cl`           |
| 5th arg                       | `r8`            | `r8b`          |
| 6th arg                       | `r9`            | `r9b`          |

	rax stores the return value of the function, look for it there. 



