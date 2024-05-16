# Shellcoding Techniques

As we have seen in the previous section, our Hello World assembly code had to be modified to produce a working shellcode. So, in this section, we'll go through some of the techniques and tricks we can use to work around any issues found in our assembly code.

## Shellcoding Requirements

As we briefly mentioned in the previous section, not all binaries give working shellcodes that can be loaded directly to the memory and run. This is because there are specific requirements a shellcode must meet. Otherwise, it won't be properly disassembled on runtime into its correct assembly instructions.

To better understand this, let's try to disassemble the shellcode we extracted in the previous section from the Hello World program, using the same pwn disasm tool we previously used:

```

pwn
$ pwn disasm '48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05' -c 'amd64'
0: 48 be 00 20 40 00 00 movabs rsi, 0x402000
7: 00 00 00
a: bf 01 00 00 00 mov edi, 0x1
f: ba 12 00 00 00 mov edx, 0x12
14: b8 01 00 00 00 mov eax, 0x1
19: 0f 05 syscall
1b: b8 3c 00 00 00 mov eax, 0x3c
20: bf 00 00 00 00 mov edi, 0x0
25: 0f 05 syscall

```

We can see that the instructions are relatively similar to the Hello World assembly code we had before, but they are not identical. We see that there's an empty line of instructions, which could potentially break the code. Furthermore, our Hello World string is nowhere to be seen. We also see many red 00's, which we'll get into in a bit.

This is what will happen if our assembly code is not shellcode compliant and does not meet the Shellcoding Requirements. To be able to produce a working shellcode, there are three main Shellcoding Requirements our assembly code must meet:

- Does not contain variables
- Does not refer to direct memory addresses
- Does not contain any NULL bytes 00

So, let's start with the Hello World program we saw in the previous section, and go through each of the above points and fix them:

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

### Remove Variables

A shellcode is expected to be directly executable once loaded into memory, without loading data from other memory segments, like .data or .bss. This is because the text memory segments are not writable, so we cannot write any variables. In contrast, the data segment is not executable, so we cannot write executable code.

So, to execute our shellcode, we must load it in the text memory segment and lose the ability to write any variables. Hence, our entire shellcode must be under '.text' in the assembly code.

Note: Some older shellcoding techniques (like the jmp-call-pop technique) no longer work with modern memory protections, as many of them rely on writing variables to the text memory segment, which, as we just discussed, is no longer possible.

There are many techniques we can use to avoid using variables, like:

- Moving immediate strings to registers
- Pushing strings to the Stack, and then use them

In the above code, we may move our string to rsi, as follows:

```nasm
    mov rsi, 'Academy!'
```

However, a 64-bit register can only hold 8 bytes, which may not be enough for larger strings. So, our other option is to rely on the Stack by pushing our string 16-bytes at a time (in reverse order), and then using rsp as our string pointer, as follows:

```nasm
    push 'y!'
    push 'B Academ'
    push 'Hello HT'
    mov rsi, rsp
```

However, this would exceed the allowed bounds of immediate strings push, which is a dword (4-bytes) at a time. So, we will instead move our string to rbx, and then push rbx to the Stack, as follows:

```nasm
    mov rbx, 'y!'
    push rbx
    mov rbx, 'B Academ'
    push rbx
    mov rbx, 'Hello HT'
    push rbx
    mov rsi, rsp
```

Note: Whenever we push a string to the stack, we have to push a 00 before it to terminate the string. However, we don't have to worry about that in this case, since we can specify the print length for the write syscall.

We can now apply these changes to our code, assemble it and run it to see if it works:

```
Shellcoding Techniques
z0x9n@htb[/htb]$ ./assembler.sh helloworld.s

Hello HTB Academy!
```

We see that it works as expected, without needing to use any variables. We can check it with gdb to see how it looks at the breakpoint:

```
gdb
$ gdb -q ./helloworld
─────────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x1
$rbx   : 0x5448206f6c6c6548 ("Hello HT"?)
$rcx   : 0x0
$rdx   : 0x12
$rsp   : 0x00007fffffffe3b8  →  "Hello HTB Academy!"
$rbp   : 0x0
$rsi   : 0x00007fffffffe3b8  →  "Hello HTB Academy!"
$rdi   : 0x1
─────────────────────────────────────────────────────────────────────────────────────────── stack ────
0x00007fffffffe3b8│+0x0000: "Hello HTB Academy!"	 ← $rsp, $rsi
0x00007fffffffe3c0│+0x0008: "B Academy!"
0x00007fffffffe3c8│+0x0010: 0x0000000000002179 ("y!"?)
───────────────────────────────────────────────────────────────────────────────────── code:x86:64 ────
→   0x40102e <_start+46>      syscall
──────────────────────────────────────────────────────────────────────────────────────────────────────
```

As we can notice, the string was built up gradually in the Stack, and when we moved rsp to rsi it contained our entire string.

### Remove Addresses

We are now not using any addresses in our above code since we removed the only address reference when we removed our only variable. However, we may see references in many cases, especially with calls or loops and such. So, we must ensure that our shellcode will know how to make the call with whatever environment it runs in.

To be able to do so, we cannot reference direct memory address (i.e. call 0xffffffffaa8a25ff), and instead only make calls to labels (i.e. call loopFib) or relative memory addresses (i.e., call 0x401020). We discussed rip-relative addressing in the gdb section.

Luckily, throughout this module, we have only been making calls to labels to ensure that we learn how to write code that is easily shellcoded. If we are making a call to a label, nasm will automatically change this label into a relative address, which should work with shellcodes.

If we ever had any calls or references to direct memory addresses, we can fix that by:

- Replacing with calls to labels or rip-relative addresses (for calls and loops)
- Push to the Stack and use rsp as the address (for mov and other assembly instructions)

If we are efficient while writing our assembly code, we may not have to fix these types of issues.

### Remove NULL

NULL characters (or 0x00) are used as string terminators in assembly and machine code, and so if they are encountered, they will cause issues and may lead the program to terminate early
