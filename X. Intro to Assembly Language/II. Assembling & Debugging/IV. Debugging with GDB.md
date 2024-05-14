# Debugging with GDB

Now that we have the general information about our program, we will start running it and debugging it. Debugging consists mainly of four steps:

| Step    | Description                                                                                                          |
| ------- | -------------------------------------------------------------------------------------------------------------------- |
| Break   | Setting breakpoints at various points of interest                                                                    |
| Examine | Running the program and examining the state of the program at these points                                           |
| Step    | Moving through the program to examine how it acts with each instruction and with user input                          |
| Modify  | Modify values in specific registers or addresses at specific breakpoints, to study how it would affect the execution |

We will go through these points in this section to learn the basics of debugging a program with GDB.

## Break

The first step of debugging is setting breakpoints to stop the execution at a specific location or when a particular condition is met. This helps us in examining the state of the program and the value of registers at that point. Breakpoints also allow us to stop the program's execution at that point so that we can step into each instruction and examine how it changes the program and values.

We can set a breakpoint at a specific address or for a particular function. To set a breakpoint, we can use the `break` or `b` command along with the address or function name we want to break at. For example, to follow all instructions run by our program, let's break at the `_start` function, as follows:

```
gef➤  b _start

Breakpoint 1 at 0x401000
```

Now, in order to start our program, we can use the `run` or `r` command:

```
gef➤  b _start
Breakpoint 1 at 0x401000
gef➤  r
Starting program: ./helloWorld

Breakpoint 1, 0x0000000000401000 in _start ()
[ Legend: Modified register | Code | Heap | Stack | String ]
───────────────────────────────────────────────────────────────────────────────────── registers ────
$rax   : 0x0
$rbx   : 0x0
$rcx   : 0x0
$rdx   : 0x0
$rsp   : 0x00007fffffffe310  →  0x0000000000000001
$rbp   : 0x0
$rsi   : 0x0
$rdi   : 0x0
$rip   : 0x0000000000401000  →  <_start+0> mov eax, 0x1
...SNIP...
───────────────────────────────────────────────────────────────────────────────────────── stack ────
0x00007fffffffe310│+0x0000: 0x0000000000000001	 ← $rsp
0x00007fffffffe318│+0x0008: 0x00007fffffffe5a0  →  "./helloWorld"
...SNIP...
─────────────────────────────────────────────────────────────────────────────────── code:x86:64 ────
     0x400ffa                  add    BYTE PTR [rax], al
     0x400ffc                  add    BYTE PTR [rax], al
     0x400ffe                  add    BYTE PTR [rax], al
 →   0x401000 <_start+0>       mov    eax, 0x1
     0x401005 <_start+5>       mov    edi, 0x1
     0x40100a <_start+10>      movabs rsi, 0x402000
     0x401014 <_start+20>      mov    edx, 0x12
     0x401019 <_start+25>      syscall
     0x40101b <_start+27>      mov    eax, 0x3c
─────────────────────────────────────────────────────────────────────────────────────── threads ────
[#0] Id 1, Name: "helloWorld", stopped 0x401000 in _start (), reason: BREAKPOINT
───────────────────────────────────────────────────────────────────────────────────────── trace ────
[#0] 0x401000 → _start()
────────────────────────────────────────────────────────────────────────────────────────────────────
```

If we want to set a breakpoint at a certain address, like `_start+10`, we can either `b *_start+10` or `b *0x40100a`:

```
gef➤  b *0x40100a
Breakpoint 1 at 0x40100a
```

The `*` tells GDB to break at the instruction stored in `0x40100a`.

Note: Once the program is running, if we set another breakpoint, like `b *0x401005`, in order to continue to that breakpoint, we should use the `continue` or `c` command. If we use `run` or `r` again, it will run the program from the start. This can be useful to skip loops, as we will see later in the module.

If we want to see what breakpoints we have at any point of the execution, we can use the `info breakpoint` command. We can also disable, enable, or delete any breakpoint. Furthermore, GDB also supports setting conditional breaks that stop the execution when a specific condition is met.

## Examine

The next step of debugging is examining the values in registers and addresses. As we can see in the previous terminal output, GEF automatically gave us a lot of helpful information when we hit our breakpoint. This is one of the benefits of having the GEF plugin, as it automates many steps that we usually take at every breakpoint, like examining the registers, the stack, and the current assembly instructions.

To manually examine any of the addresses or registers or examine any other, we can use the `x` command in the format of `x/FMT ADDRESS`, as `help x` would tell us. The `ADDRESS` is the address or register we want to examine, while `FMT` is the examine format. The examine format `FMT` can have three parts:

| Argument | Description                                        | Example                                          |
| -------- | -------------------------------------------------- | ------------------------------------------------ |
| Count    | The number of times we want to repeat the examine  | 2, 3, 10                                         |
| Format   | The format we want the result to be represented in | x(hex), s(string), i(instruction)                |
| Size     | The size of memory we want to examine              | b(byte), h(halfword), w(word), g(giant, 8 bytes) |

### Instructions

For example, if we wanted to examine the next four instructions in line, we will have to examine the `$rip` register (which holds the address of the next instruction), and use `4` for the count, `i` for the format, and `g` for the size (for 8-bytes or 64-bits). So, the final examine command would be `x/4ig $rip`, as follows:

```
gef➤  x/4ig $rip

=> 0x401000 <_start>:	mov    eax,0x1
   0x401005 <_start+5>:	mov    edi,0x1
   0x40100a <_start+10>:	movabs rsi,0x402000
   0x401014 <_start+20>:	mov    edx,0x12
```

We see that we get the following four instructions as expected. This can help us as we go through a program in examining certain areas and what instructions they may contain.

### Strings

We can also examine a variable stored at a specific memory address. We know that our message variable is stored at the `.data` section on address `0x402000` from our previous disassembly. We also see the upcoming command `movabs rsi, 0x402000`, so we may want to examine what is being moved from `0x402000`.

In this case, we will not put anything for the Count, as we only want one address (1 is the default), and will use `s` as the format to get it in a string format rather than in hex:

```
gef➤  x/s 0x402000

0x402000:	"Hello HTB Academy!"
```

As we can see, we can see the string at this address represented as text rather than hex characters.

Note: if we don't specify the Size or Format, it will default to the last one we used.

### Addresses

The most common format of examining is hex `x`. We often need to examine addresses
