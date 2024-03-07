# File Descriptors and Redirections in Linux

## File Descriptors

In Unix/Linux operating systems, a **file descriptor (FD)** is an indicator of the connection maintained by the kernel for Input/Output (I/O) operations. The first three file descriptors are:

- **STDIN (0):** Data stream for input.
- **STDOUT (1):** Data stream for output.
- **STDERR (2):** Data stream for output related to an error occurring.

### STDIN and STDOUT Example

When using the `cat` command, we provide input through STDIN (FD 0) by typing "SOME INPUT" and press [ENTER]. The program returns the input to the terminal as STDOUT (FD 1).

![alt text](/Images/image-2.png)

### STDOUT and STDERR Example

With the `find` command, STDOUT (FD 1) displays standard output (marked in green), while STDERR (FD 2) displays standard error (marked in red).

```bash
$ find /etc/ -name shadow
```

![alt text](/Images/image-3.png)

Redirecting STDERR to "/dev/null" discards errors:

```bash
$ find /etc/ -name shadow 2>/dev/null
```

![alt text](/Images/image-4.png)

## Redirection Examples

### Redirect STDOUT to a File

Redirect STDOUT to a file named `results.txt`:

```bash
$ find /etc/ -name shadow 2>/dev/null > results.txt
```

### Redirect STDOUT and STDERR to Separate Files

Redirect STDOUT and STDERR to different files:

```bash
$ find /etc/ -name shadow 2> stderr.txt 1> stdout.txt
```

### Redirect STDIN

Use the `<` symbol to redirect STDIN. Example using the contents of `stdout.txt` as STDIN for the `cat` command:

```bash
$ cat < stdout.txt
```

### Redirect STDOUT and Append to a File

Use `>>` to append STDOUT to an existing file:

```bash
$ find /etc/ -name passwd >> stdout.txt 2>/dev/null
```

### Redirect STDIN Stream to a File

Use `<<` to redirect a stream as STDIN. Example using the `cat` command to read streaming input and direct it to `stream.txt`:

```bash
$ cat << EOF > stream.txt
```

## Pipes

Use pipes (`|`) to redirect STDOUT from one program to another. Example using `grep` to filter lines containing "systemd" from the results of the `find` command:

```bash
$ find /etc/ -name *.conf 2>/dev/null | grep systemd
```

Multiple redirections can be used in a sequence. Example using `wc` to count the total number of lines obtained:

```bash
$ find /etc/ -name *.conf 2>/dev/null | grep systemd | wc -l
```
