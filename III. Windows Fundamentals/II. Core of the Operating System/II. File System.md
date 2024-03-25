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

```bash
C:\htb> icacls c:\windows
```

This command displays permissions for the Windows directory.

Basic access permissions include:

- F : Full access
- D : Delete access
- N : No access
- M : Modify access
- RX : Read and execute access
- R : Read-only access
- W :  Write-only access

We can add and remove permissions using `icacls`, granting or revoking access for specific users or groups. This utility is powerful and useful for managing file permissions efficiently.
