# Syscalls

Even though we are talking directly to the CPU through machine instructions in Assembly, we do not have to invoke every type of command using basic machine instructions only. Programs regularly use many kinds of operations. The Operating System can help us through syscalls to not have to execute these operations every time manually.

For example, suppose we need to write something on the screen, without syscalls. In that case, we will need to talk to the Video Memory and Video I/O, resolve any encoding required, send our input to be printed, and wait for the confirmation that it has been printed. As expected, if we had to do all of this to print a single character, it would make assembly codes much longer.

## Linux Syscall

A syscall is like a globally available function written in C, provided by the Operating System Kernel. A syscall takes the required arguments in the registers and executes the function with the provided arguments. For example, if we wanted to write something to the screen, we can use the write syscall, provide the string to be printed and other required arguments, and then call the syscall to issue the print.

There are many available syscalls provided by the Linux Kernel, and we can find a list of them and the syscall number of each one by reading the `unistd_64.h` system file:

```
z0x9n@htb[/htb]$ cat /usr/include/x86_64-linux-gnu/asm/unistd_64.h
#ifndef _ASM_X86_UNISTD_64_H
#define _ASM_X86_UNISTD_64_H 1

#define __NR_read 0
#define __NR_write 1
#define __NR_open 2
#define __NR_close 3
#define __NR_stat 4
#define __NR_fstat 5
```

The above file sets the syscall number for each syscall to refer to that syscall using this number.

Note: With 32-bit x86 processors, the syscall numbers are in the `unistd_32.h` file.

Let's practice using syscalls with the `write` syscall that prints to the screen. We will not be printing the Fibonacci numbers just yet, but will instead start by printing an intro message, `Fibonacci Sequence`, at the beginning of our program.

## Syscall Function Arguments

To use the `write` syscall, we must first know what arguments it accepts. To find the arguments accepted by a syscall, we can use the `man -s 2` command with the syscall name from the above list:

```
z0x9n@htb[/htb]$ man -s 2 write
...SNIP...
       ssize_t write(int fd, const void *buf, size_t count);
```

As we can see from the above output, the `write` function has the following syntax:

```c
ssize_t write(int fd, const void *buf, size_t count);
```

We see that the syscall function expects 3 arguments:

- File Descriptor `fd` to be printed to (usually 1 for stdout)
- The address pointer to the string to be printed
- The length we want to print

Once we provide these arguments, we can use the `syscall` instruction to execute the function and print to screen. In addition to these manual methods of locating syscalls and function arguments, there are online resources we can use to quickly look for syscalls, their numbers, and the arguments they expect, like [this table](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/). Furthermore, we can always refer to the Linux source code on [Github](https://github.com/torvalds/linux).

Tip: The `-s 2` flag specifies syscall man pages. We can check `man man` to see various sections for each man page.

## Syscall Calling Convention

Now that we understand how to locate various syscall and their arguments let's start learning how to call them. To call a syscall, we have to:

- Save registers to stack
- Set its syscall number in `rax`
- Set its arguments in the registers
- Use the `syscall` assembly instruction to call it

We usually should save any registers we use to the stack before any function call or syscall. However, as we are running this syscall at the beginning of our program before using any registers, we don't have any values in the registers, so we should not worry about saving them.
We will discuss saving registers to the stack when we get to Function Calls.

### Syscall Number

Let's start by moving the syscall number to the `rax` register. As we saw earlier, the `write` syscall has a number 1, so we can start with the following command:

```nasm
mov rax, 1
```

Now, if we reach the `syscall` instruction, the Kernel would know which syscall we are calling.

### Syscall Arguments

Next, we should put each of the function's arguments in its corresponding register. The x86_64 architecture's calling convention specifies in which register each argument should be placed (e.g., first arg should be in `rdi`). All functions and syscalls should follow this standard and take their arguments from the corresponding registers. We have discussed the following table in the Registers section:

| Description                 | 64-bit Register | 8-bit Register |
| --------------------------- | --------------- | -------------- |
| Syscall Number/Return value | rax             | al             |
| Callee Saved                | rbx             | bl             |
| 1st arg                     | rdi             | dil            |
| 2nd arg                     | rsi             | sil            |
| 3rd arg                     | rdx             | cl             |
| 4th arg                     | rcx             | bpl            |
| 5th arg                     | r8              | r8b            |
| 6th arg                     | r9              | r9b            |

As we can see, we have a register for each of the first 6 arguments. Any additional arguments can be stored in the stack (though not many syscalls use more than 6 arguments.).

Note: `rax` is also used for storing the return value of a syscall or a function. So, if we were expecting to get a value back from a syscall/function, it will be in `rax`.

With that, we should know our arguments and in which register we should store them. Going back to the `write` syscall function, we should pass: `fd`, `pointer`, and `length`. We can do so as follows:

- `rdi` -> 1 (for stdout)
- `rsi` -> `'Fibonacci Sequence:\n'` (pointer to our string)
- `rdx` -> 20 (length of our string)

We can use `mov rcx, 'string'`. However, we can only store up to 16 characters in a register (i.e., 64 bits), so our intro string would not fit. Instead, let's create a variable with our string (as we learned in the Assembly File Structure section), similarly to what we did with the Hello World program:

```nasm
global  _start

section .data
    message db "Fibonacci Sequence:", 0x0a
```

Note how we added `0x0a` after our string, to add a new line character.

The `message` label is a pointer to where our string will be stored in the memory. So, we can use it as our second argument. So, our final syscall code should be as follows:

```nasm
mov rax, 1       ; rax: syscall number 1
mov rdi, 1       ; rdi: fd 1 for stdout
mov rsi, message ; rsi: pointer to message
mov
```
