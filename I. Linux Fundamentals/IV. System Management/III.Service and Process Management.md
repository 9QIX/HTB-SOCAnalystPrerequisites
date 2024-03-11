# Service and Process Management

In general, there are two types of services: **internal** and **user-installed**. Internal services are required at system startup, performing hardware-related tasks. User-installed services, also known as daemons, run in the background without user interaction and usually include server services like sshd or systemd.

Most Linux distributions have switched to **systemd**, an Init process started first with PID 1. It orderly starts and stops other services. Processes have PIDs and can be viewed under `/proc/`. A process can also have a Parent Process ID (PPID) if it is a child process.

## systemctl

After installing OpenSSH on a VM, start the service with:

```bash
z0x9n@htb[/htb]$ systemctl start ssh
```

Check its status:

```bash
z0x9n@htb[/htb]$ systemctl status ssh
```

To add OpenSSH to the SysV script and run it after startup:

```bash
z0x9n@htb[/htb]$ systemctl enable ssh
```

After a system reboot, the OpenSSH server will automatically run. Check with:

```bash
z0x9n@htb[/htb]$ ps -aux | grep ssh
```

List all services with systemctl:

```bash
z0x9n@htb[/htb]$ systemctl list-units --type=service
```

If services don't start due to an error, use `journalctl` to view logs:

```bash
z0x9n@htb[/htb]$ journalctl -u ssh.service --no-pager
```

## Kill a Process

Processes can be in various states. Use `kill`, `pkill`, `pgrep`, and `killall`. View all signals:

```bash
z0x9n@htb[/htb]$ kill -l
```

Common signals include:

- SIGHUP
- SIGINT
- SIGQUIT
- SIGKILL
- SIGTERM

For example, to force kill a frozen program:

```bash
z0x9n@htb[/htb]$ kill 9 <PID>
```

## Background a Process

Suspend a process with [Ctrl + Z], view background processes with `jobs`, and resume with `bg`:

```bash
z0x9n@htb[/htb]$ bg
```

Another option is to use the '&' sign at the end of the command:

```bash
z0x9n@htb[/htb]$ ping -c 10 www.hackthebox.eu &
```

## Foreground a Process

List background processes with `jobs` and bring a process to the foreground with `fg <ID>`:

```bash
z0x9n@htb[/htb]$ fg 1
```

## Execute Multiple Commands

Use semicolons (;) to separate commands, double ampersands (&&) to run commands sequentially with error checking:

```bash
z0x9n@htb[/htb]$ echo '1'; echo '2'; echo '3'
```

If there's an error in a command using &&, subsequent commands won't execute:

```bash
z0x9n@htb[/htb]$ echo '1' && ls MISSING_FILE && echo '3'
```

Pipes (|) depend on both correct operation and results of previous processes.
