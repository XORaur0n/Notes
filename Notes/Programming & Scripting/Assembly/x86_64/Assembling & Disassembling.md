
Save assembly code as .asm or .sh 

Once assembled, you'll need to link it - below is a simple bash script for this 

```
```bash
#!/bin/bash

fileName="${1%%.*}" # remove .s extension

nasm -f elf64 ${fileName}".s"
ld ${fileName}".o" -o ${fileName}
[ "$2" == "-g" ] && gdb -q ${fileName} || ./${fileName}
```
```
