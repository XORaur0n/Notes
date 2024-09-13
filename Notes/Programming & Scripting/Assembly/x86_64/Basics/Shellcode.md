
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



To dump from a specific section/function use: 

```python

>>> file.section("[section/function]").hex()

```