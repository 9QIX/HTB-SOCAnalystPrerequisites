# **Skills Assessment**

## **Task 1**

We are contracting for a company, and they find a suspicious binary file. We examine the file with `gdb` and see that it is loading an encoded shellcode to the Stack and storing the `xor` decoding key in `rbx`. We need to decode the shellcode after it is loaded to the Stack and then run the shellcode to get the flag.

### Tips

1. Refer to the "`Assembling & Disassembling`" section to get the assembly code of the binary. We can also use the template from that section. Do not forget to change `movabs` to `mov`.

2. Refer to the "`Arithmetic Instructions`" and "`Loops`" sections to write instructions to decode the shellcode after it is loaded to the Stack.

3. Refer to the "`GDB`" section to examine the entire shellcode once it is decoded.

4. Refer to the "`Shellcodes`" section to be able to run the decoded shellcode.

## **Task 2**

We are performing a pentest, and in a binary exploitation exercise, we reach the point where we have to run our shellcode. However, only a buffer space of 50 bytes is available to us. So, we have to optimize our assembly code to make it shellcode-ready and under 50-bytes to successfully run it on the vulnerable server.

### Tips

1. Refer to the "`Syscalls`" section to understand what the assembly code is doing.

2. Refer to the "`Shellcoding Techniques`" section to be able to optimize the assembly code.
