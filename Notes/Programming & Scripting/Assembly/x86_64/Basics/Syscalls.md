
Designed to be able to execute operations through the CPU without writing machine code manually each time. 


Think of `syscalls` as globally available `functions` provided by the OS. `Syscalls` take the arguments from the registers and execute the function with those arguments

To see all available `syscalls` on Linux: 

For 64-bit: 

	/usr/include/x86_64-linux-gnu/asm/unistd_64.h

For 32-bit: 

	/usr/include/x86_64-linux-gnu/asm/unistd_32.h

To see available `syscalls` on Windows: 

	Refer to Windows API documentation, use tools (Procmon, WinDbg, Ghidra/IDA, etc.) 




