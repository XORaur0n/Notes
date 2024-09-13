
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



