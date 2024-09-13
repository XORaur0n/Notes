
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

Once installed use the below to get the machine code for an instruction and vice/versa: 

```shell

#to assemble the code and get the shellcode

pwn asm '[instruction]' -c '[architecture]'

#to disassemble the shellcode and get the raw instruction(s)

pwn disasm '[machine code]' -c '[architecture]'


```


-------------------------------------------

**Extracting Shellcode:** 

To read a binary:

```python
#from python3 interpreter
>>>from pwn import *
>>> file = [binary type](binary name)

```



Next, to dump from a specific section/function use: 

```python
#from python3 interpreter
>>> file.section("[section/function]").hex()

```


Python script to extract shellcode of any binary: 

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
#!bin/bash

for i in $(objdump -d $1 |grep "^ " |cut -f2); do echo -n $i; done; echo;

```


-------------------------------------------

**Loading Shellcode:** 

The following will turn the shellcode back into binary and run it:

```python

#from python3 interpreter

>>> from pwn import *
>>> context(os="[OS]" , arch="[architecture]" , log_level="error")
>>> run_shellcode(unhex('[shellcode]')).interactive()

```


**Debugging Shellcode:** 

The following will convert the shellcode into an executable ELF format:

```python
#from python3 interpreter
>>> from pwn import *
>>> ELF.from_bytes(unhex('[shellcode]')).save('[file name]')

```


-------------------------------------------


**Requirements**: 

In order to produce working shellcode it must meet the following criteria: 

1. Doesn't contain `variables`
2. Doesn't refer to direct memory `addresses`
3. Doesn't contain any NULL bytes `00` 

-------------------------------------------

**Removing Variables:** 

Since shellcode is supposed to be directly executable once loaded into memory (without loading data from other segments like `.data or .bss`.  This is because it is normally blocked by `Data Execution Prevention (DEP)`

The `.text` segments are not writable, as in we cannot write any variables to it. The `.data` segments are not executable, as in we cannot write executable code to it. 

So, in order to run shellcode it must be written into the `.text` segment.

In order to avoid using variables you can: 

1. Move immediate `strings` to `registers`

```nasm
mov rsi 'string'
```

2. `Push` `strings` onto the `stack` and then use them

```nasm
; You could try it this way, pushing the string onto the stack 16-bytes at a time, backwards...

    push 'y!'
    push 'B Academ'
    push 'Hello HT'
    mov rsi, rsp

; But this exceeds the amount allowed by immediate strings push (dword's bound is 4-bytes)

; Below is the way to bypass this, move the string into `rbx` and then push `rbx` onto the `Stack`

 mov rbx, 'y!'
    push rbx
    mov rbx, 'B Academ'
    push rbx
    mov rbx, 'Hello HT'
    push rbx
    mov rsi, rsp
    
```

	When we push a string to the stack a 00 needs to be pushed before it (remember backwards!) to terminate the string (unless you can specify the print length for a write syscall)

-------------------------------------------

**Removing Addresses:** 

We aren't using any `addresses` in any of the shellcode, but we may see references (`calls & loops)`, so we have to make sure that the shellcode will know how to make a call with whatever environment it runs in.

To do this we cannot reference direct `addresses` (`call 0xffffffffaa8a25ff`), but we can make calls to labels (`call loopName)` or relative `addresses` (`call 0x401020`)

If any calls/references to direct addresses come up, we can remedy them by: 

1. Replacing with `calls` to labels or` rip-relative addresses` (for `calls` and `loops`)

2. Push to the `Stack` and use `rsp` as the `address` (for `mov` and other assembly instructions)

-------------------------------------------

**Removing NULL:** 

`NULL` characters (`0x00`) are used to terminate s
