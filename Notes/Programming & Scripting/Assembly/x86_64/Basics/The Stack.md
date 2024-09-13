
A segment of memory allocated to store data and is usually used to also retrieve that data temporarily.

The top of the stack is referred to by `rsp` and the bottom is by `rbp` 

Data can moved to the top of the stack with `push` 
or moved out of the stack to a memory address/register with `pop` 

The Stack is designed to be last-in-first-out (LIFO), meaning that only the most recent element can be moved off. 

	If rax is pushed onto the stack, the top of the stack is now the value of rax
	If anything is pushed on top of rax, it will need to be popped off before you can pop the originial value back into rax

	When pushing & popping things into & out of the stack, we need to do them i

-------------------------------------------

**Usage**: 

We will primarily be pushing data to the stack before we call a `function` or make a `syscall`, and then restore them after they are complete. 

This is because `functions` and `syscalls` mostly use registers for processing, so if values stored within the register are changed once they're over, we will lose the data. 



