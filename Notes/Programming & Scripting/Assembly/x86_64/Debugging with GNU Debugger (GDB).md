
Install GEF plugin: 

	wget -O ~/.gdbinit-gef.py -q https://gef.blah.cat/py

	echo source ~/.gdbinit-gef.py >> ~/.gdbinit

-------------------------------------------

**Steps of Debugging:** 

| Technique | Description |
|-----------|-------------|
| **Break** | Setting breakpoints at various points of interest in the code to pause execution and inspect the program's state. |
| **Examine** | Running the program and examining its state at breakpoints, including checking variable values, memory contents, and the call stack. |
| **Step** | Moving through the program one instruction or line of code at a time to observe its behavior and how user input affects it. |
| **Modify** | Changing values in specific registers or memory addresses at breakpoints to study how these modifications impact program execution. |

-------------------------------------------

**Break:** 

Breakpoints are set when specific parameters are met so we can examine the program & register values at that particular point. This allows for stepping into instructions to determine how they change the program & values. 

You can set breakpoints at specific addresses or functions like this within the gef prompt: 

	b _[function]

To run the code to the break:

	r

Can also set break at addresses like: 

	b *0x40100a

	b _start+10

-------------------------------------------

**Examine:** 

To manually examine any address or registers we can use: 

	x/[FMT] [Address]


Examine Format for FMT:

| Argument   | Description                                               | Example                                                      |
| ---------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| **Count**  | The number of times we want to repeat the examination.    | `2`, `3`, `10`                                               |
| **Format** | The format in which we want the result to be represented. | `x` (hexadecimal), `s` (string), `i` (instruction)           |
| **Size**   | The size of memory we want to examine.                    | `b` (byte), `h` (halfword), `w` (word), `g` (giant, 8 bytes) |

Instructions:

In order to see the next 4 instructions for instance, we will need to examine the rip register (which holds the next instruction), use `4` for the count, `i` for the format, and `g` for the size. The whole command as follows:

	x/4ig $rip


Strings: 

To pull strings use: 

	x/s [address]


Addresses: 

To look at hex use: 

	x/wx [address]

Also use the following to see current values in registers: 

	registers

-------------------------------------------

**Step:** 

To step through each instruction one by one use: 

	si

To step ahead a certain number of instructions use the above followed by a number

To continue through the function you're on until another one is called or the function breaks at the end of the program use: 

	step


-------------------------------------------

**Modify:** 

