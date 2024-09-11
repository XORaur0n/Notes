
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

ob

