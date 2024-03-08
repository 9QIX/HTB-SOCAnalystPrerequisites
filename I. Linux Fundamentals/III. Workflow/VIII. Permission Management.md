# Permission Management

Under Linux, permissions are assigned to users and groups. Each user can be a member of different groups, and membership in these groups gives the user specific, additional permissions. Each file and directory belongs to a specific user and a specific group. When creating new files or directories, they belong to the group we are part of.

When a user wants to access the contents of a Linux directory, they must first traverse the directory, requiring execute permissions on the directory. Without this permission, users cannot access the directory's contents and will be presented with a “Permission Denied" error message.

```bash
$ ls -l

drw-rw-r-- 3 cry0l1t3 cry0l1t3   4096 Jan 12 12:30 scripts

$ ls -al mydirectory/

ls: cannot access 'mydirectory/script.sh': Permission denied
ls: cannot access 'mydirectory/..': Permission denied
ls: cannot access 'mydirectory/subdirectory': Permission denied
ls: cannot access 'mydirectory/.': Permission denied
total 0
d????????? ? ? ? ?            ? .
d????????? ? ? ? ?            ? ..
-????????? ? ? ? ?            ? script.sh
d????????? ? ? ? ?            ? subdirectory
```

Execute permissions are necessary to traverse a directory, irrespective of the user's level of access. Execute permissions on a directory do not allow a user to execute or modify any files within the directory, only to traverse and access the content.

To execute files within the directory, a user needs execute permissions on the corresponding file. To modify the contents of a directory, the user needs write permissions on the directory.

The Linux permission system is based on the octal number system, with three different types of permissions for files or directories:

- (r) Read
- (w) Write
- (x) Execute

Permissions can be set for the owner, group, and others as shown below:

```bash
$ ls -l /etc/passwd

- rwx rw- r--   1 root root 1641 May  4 23:42 /etc/passwd
```

- Binary Notation: `4 2 1 | 4 2 1 | 4 2 1`
- Binary Representation: `1 1 1 | 1 0 1 | 1 0 0`
- Octal Value: `7 | 5 | 4`
- Permission Representation: `r w x | r - x | r - -`

If we sum the set bits from the Binary Representation assigned to the values from Binary Notation together, we get the Octal Value. The Permission Representation represents the bits set in the Binary Representation using three characters.

## Change Permissions

We can modify permissions using the `chmod` command, permission group references (`u` - owner, `g` - Group, `o` - others, `a` - All users), and either a [+] or a [-] to add or remove the designated permissions.

```bash
$ ls -l shell

-rwxr-x--x   1 cry0l1t3 htbteam 0 May  4 22:12 shell
```

To apply read permissions for all users:

```bash
$ chmod a+r shell && ls -l shell

-rwxr-xr-x   1 cry0l1t3 htbteam 0 May  4 22:12 shell
```

Setting permissions for all other users to read-only using octal value assignment:

```bash
$ chmod 754 shell && ls -l shell

-rwxr-xr--   1 cry0l1t3 htbteam 0 May  4 22:12 shell
```

- Binary Notation: `4 2 1 | 4 2 1 | 4 2 1`
- Binary Representation: `1 1 1 | 1 0 1 | 1 0 0`
- Octal Value: `7 | 5 | 4`
- Permission Representation: `r w x | r - x | r - -`

### Cheat Sheet

| Bin | Decimal | Representation |
| --- | ------- | -------------- |
| 000 | 0       | - - -          |
| 001 | 1       | - - x          |
| 010 | 2       | - w -          |
| 011 | 3       | - w x          |
| 100 | 4       | r - -          |
| 101 | 5       | r - x          |
| 110 | 6       | r w -          |
| 111 | 7       | r w x          |

## Change Owner

To change the owner and/or the group assignments of a file or directory, use the `chown` command.

```bash
$ chown root:root shell && ls -l shell

-rwxr-xr--   1 root root 0 May  4 22:12 shell
```

## SUID & SGID

Besides assigning direct user and group permissions, you can configure special permissions for files by setting the Set User ID (SUID) and Set Group ID (SGID) bits. The letter "s" is used instead of an "x".

Administrators often use SUID/SGID bits to give users special rights for certain applications or files. However, this can pose a security risk if not managed properly.

## Sticky Bit

Sticky bits are a type of file permission in Linux set on directories. They provide extra security when controlling the deletion and renaming of files within a directory, typically used on directories shared by multiple users.

For example, in a shared home directory, a system administrator can set the sticky bit on the directory to ensure that only the owner, directory owner, or root user can delete or rename files within the directory.

```bash
$ ls -l

drw-rw-r-t 3 cry0l1t3 cry0l1t3   4096 Jan 12 12:30 scripts
drw-rw-r-T 3 cry0l1t3 cry0l1t3   4096 Jan 12 12:32 reports
```

Here, both directories have the sticky bit set. The uppercase T means other users do not have execute permissions, while the lowercase t means execute permissions are set.
