
**Assembling:**

Save assembly code as .asm or .sh 

Once assembled, you'll need to assemble & link it - below is a simple bash script for this 

```bash
#!/bin/bash

fileName="${1%%.*}" # remove .s extension

nasm -f elf64 ${fileName}".s"
ld ${fileName}".o" -o ${fileName}
[ "$2" == "-g" ] && gdb -q ${fileName} || ./${fileName}
```

Alternatively you could link it yourself by assembling it first

	nasm -f elf64 [filename]

The above will assemble the file and output a [filename].o. 

	ld -o [new filename] [filename].o

The above will link the file and make it executable

-----------------------------------------

Disassembling: 

objdump is best for this: 

The below will output the assembly, the addresses, and the machine code. 

	objdump -M intel -d [filename]
	
The below will do the same, but only show the assembly. 

	objdump -M intel --no-show-raw-insn --no-addresses -d [filename]

Sometimes you may other fields from the disassembled file, as the -d flag only gives us .text

The below will will dump strings from the .data section of the assembly

	objdump -sj .data [filename]


