# File System 📁

There are 5 types of Windows file systems:

- FAT12
- FAT16
- FAT32
- NTFS
- exFAT

FAT12 and FAT16 are no longer used on modern Windows operating systems. Let's delve into the FAT32 and NTFS file systems:

#### FAT32

- **Pros**:
  - Device compatibility
  - Operating system cross-compatibility
- **Cons**:
  - Limited to files under 4GB in size
  - Lacks built-in data protection or compression features

#### NTFS

- **Pros**:
  - Reliable with system failure recovery
  - Granular security permissions
  - Supports large partitions
  - Built-in journaling for file modifications
- **Cons**:
  - Limited support on mobile devices
  - Older media devices might not support NTFS

### Permissions

NTFS offers various permissions:

| Permission Type      | Description                                                        |
| -------------------- | ------------------------------------------------------------------ |
| Full Control         | Reading, writing, changing, deleting files/folders                 |
| Modify               | Reading, writing, and deleting files/folders                       |
| List Folder Contents | Viewing, listing folders and subfolders, executing files           |
| Read and Execute     | Viewing, listing files and subfolders, executing files             |
| Write                | Adding files to folders, writing to a file                         |
| Read                 | Viewing, listing folders and subfolders, viewing a file's contents |
| Traverse Folder      | Allows moving through folders to reach other files or folders      |

Files and folders inherit permissions from their parent folder. We can manage NTFS permissions using the `icacls` command-line utility.

### Integrity Control Access Control List (icacls)

We can list NTFS permissions using `icacls`. For example:

```ps1
C:\htb> icacls c:\windows

c:\windows NT SERVICE\TrustedInstaller:(F)
           NT SERVICE\TrustedInstaller:(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(M)
           NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
           BUILTIN\Administrators:(M)
           BUILTIN\Administrators:(OI)(CI)(IO)(F)
           BUILTIN\Users:(RX)
           BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
           CREATOR OWNER:(OI)(CI)(IO)(F)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

The resource access level is listed after each user in the output. The possible inheritance settings are:

1. `(CI): container inherit`:

   - This indicates that the permission is inherited by subfolders within the current folder.
   - If a folder has this attribute set, any new subfolders created within it will inherit this permission.

2. `(OI): object inherit`:

   - This indicates that the permission is inherited by files within the current folder.
   - If a folder has this attribute set, any new files created within it will inherit this permission.

3. `(IO): inherit only`:

   - This indicates that the permission is inherited but not directly applied to the current object.
   - It's typically used when an object (file or folder) is created within a container that has inheritable permissions, but the object itself doesn't directly receive those permissions.

4. `(NP): do not propagate inherit`:

   - This indicates that inheritance is blocked and the permission won't be inherited by child objects.
   - It's used to prevent permissions from cascading down to subfolders or files within a folder.

5. `(I): permission inherited from parent container`:
   - This indicates that the permission is inherited from the parent folder or container.
   - It's shown to denote that the permission was not directly set on the current object but rather inherited from a higher-level container.

In the above example, the NT AUTHORITY\SYSTEM account has object inherit, container inherit, inherit only, and full access permissions. This means that this account has full control over all file system objects in this directory and subdirectories.

Basic access permissions are as follows:

- F : Full access
- D : Delete access
- N : No access
- M : Modify access
- RX : Read and execute access
- R : Read-only access
- W :  Write-only access

We can add and remove permissions using `icacls`, granting or revoking access for specific users or groups. This utility is powerful and useful for managing file permissions efficiently.

```ps1
C:\htb> icacls c:\Users
c:\Users NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         BUILTIN\Users:(RX)
         BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
         Everyone:(RX)
         Everyone:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

Using the command icacls c:\users /grant joe:f we can grant the joe user full control over the directory, but given that (oi) and (ci) were not included in the command, the joe user will only have rights over the c:\users folder but not over the user subdirectories and files contained within them.

```ps1
C:\htb> icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files
```

```ps1
C:\htb> >icacls c:\users
c:\users WS01\joe:(F)
         NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         BUILTIN\Users:(RX)
         BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
         Everyone:(RX)
         Everyone:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

These permissions can be revoked using the command icacls c:\users /remove joe.

icacls is very powerful and can be used in a domain setting to give certain users or groups specific permissions over a file or folder, explicitly deny access, enable or disable inheritance permissions, and change directory/file ownership.

A full listing of icacls command-line arguments and detailed permission settings can be found here.

#### Questions

What system user has full control over the c:\users directory?

```ps1
icacls c:\users
```

```ps1
.Everyone:(0I)(CI)(RX)
 NT AUTHORITY\SYSTEM:(0I)(CI)(F)
 BUILTIN\Administrators:(0I)(CI)(F)
 WS01\bob.smith:(0I)(CI)(F)
 BUILTIN\Users:(0I)(CI)(RX)
```

```ps1
bob.smith
```
