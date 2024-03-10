# User Management

User management is a crucial aspect of Linux administration. Tasks may involve creating new users, adding users to specific groups, or executing commands as different users. Sometimes, specific users or groups have permissions to access or modify particular files or directories. Let's explore an example of executing commands as a different user.

## Execution as a User

```bash
z0x9n@htb[/htb]$ cat /etc/shadow

cat: /etc/shadow: Permission denied
```

## Execution as Root

```bash
z0x9n@htb[/htb]$ sudo cat /etc/shadow

root:<SNIP>:18395:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
<SNIP>
```

Here is a list of commands to help understand and manage users effectively:

| Command    | Description                                                                                                            |
| ---------- | ---------------------------------------------------------------------------------------------------------------------- |
| `sudo`     | Execute command as a different user.                                                                                   |
| `su`       | The `su` utility requests appropriate user credentials via PAM and switches to that user ID. A shell is then executed. |
| `useradd`  | Creates a new user or updates default new user information.                                                            |
| `userdel`  | Deletes a user account and related files.                                                                              |
| `usermod`  | Modifies a user account.                                                                                               |
| `addgroup` | Adds a group to the system.                                                                                            |
| `delgroup` | Removes a group from the system.                                                                                       |
| `passwd`   | Changes user password.                                                                                                 |

User management is crucial in any operating system. The best way to become familiar with it is to try out the individual commands with their various options. Experimenting with these commands will enhance your understanding of user management in Linux.
