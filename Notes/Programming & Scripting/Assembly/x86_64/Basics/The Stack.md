
A segment of memory allocated to store data and is usually used to also retrieve that data temporarily.

The top of the stack is referred to by `rsp` and the bottom is by `rbp` 

Data can moved to the top of the stack with `push` 
or moved out of the stack to a memory address/register with `pop` 

The Stack is designed to be last-in-first-out (LIFO), meaning that only the most recent element can be moved off. 

	if rax is pushed onto the stack, the top of the stack is now the value of rax
	If anything is pushed on top of rax, it will need to be popped off 