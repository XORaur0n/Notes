
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

Being able to inject shellcode directly into memory and have it executed is a big part of binary exploitation. 


