
A hex representation of a binary's executable machine code

A simple Hello World program: 

```nasm
global _start

section .data
    message db "Hello HTB Academy!"

section .text
_start:
    mov rsi, message
    mov rdi, 1
    mov rdx, 18
    mov rax, 1
    syscall

    mov rax, 60
    mov rdi, 0
    syscall

``` 

Its shellcode: 

```shellcode

48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05
```

-------------------------------------------

**Use in Penetration Testing:**

Being able to inject shellcode directly into memory and have it executed is a big part of binary exploitation. Given that nothing is written to disk, it is much harder to detect

Modern x86_64 systems have protections for doing this directly, but Return-Oriented Programming/Chaining are used to defeat these safeguards

Also, many attacks rely on existing executables (`elf or .exe)` or libraries like (`.so or .dll)` to run before shellcode can be executed. 

-------------------------------------------

**Assembly into Machine Code:** 

Each x86 `instruction` & each `register` has its own binary machine code (usually in hex), which represents the binary code the CPU will execute.

Common combinations of `instructions` & `registers` have their own machine code too. When the x86 is assembled, its converted into respective machine code for the CPU to execute


Determining Machine Codes: 

Once installed use the below to get the machine code for an instruction: 

	pwn asm '[instruction]' -c '[architecture]'


-------------------------------------------

**Extracting Shellcode:** 

To read a binary:

```python
>>>from pwn import *
>>> file = [binary type](binary name)

```



Next, to dump from a specific section/function use: 

```python

>>> file.section("[section/function]").hex()

```


Python script to extract shellcode of any binary: 

```
```
```python

#!/usr/bin/python3

import sys
from pwn import *

context(os="linux", arch="amd64", log_level="error")

file = ELF(sys.argv[1])
shellcode = file.section(".text")
print(shellcode.hex())


```


Bash script to do the same (with objdump):

```bash

for i in $(objdump -d $1 |grep "^ " |cut -f2); do echo -n $i; done; echo;

```


-------------------------------------------

**Loading Shellcode:** 

The following will turn the shellcode back into binary and run it:

```python

>>> from pwn import *
>>> context(os="[OS]" , arch="[architecture]" , log_level="error")
>>> run_shellcode(unhex('[shellcode]')).interactive()

```


**Debugging Shellcode:** 

The following will convert the shellcode into an executable ELF format:

```python
>>> from pwn import *
>>> ELF.from_bytes(unhex('[shellcode]')).save('[file name]')

```