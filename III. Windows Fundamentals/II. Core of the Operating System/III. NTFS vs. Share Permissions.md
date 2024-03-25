### NTFS vs. Share Permissions

Microsoft dominates over 70% of the global market share in desktop operating systems with Windows. Consequently, Windows is a prime target for malware authors due to its widespread usage and high value as a target. Despite common misconceptions, no operating system is immune to malware. Malware can be developed for any OS, including viruses with malicious intent.

![alt text](/Images/image-14.png)

#### SMB Protocol and Windows Shares

The Server Message Block (SMB) protocol facilitates shared resource connections like files and printers in Windows environments, used across enterprises of all sizes.

**NTFS Permissions vs. Share Permissions**

NTFS permissions and share permissions, while often used on the same shared resource, serve different purposes. Let's explore the individual permissions for securing network shares on Windows systems with the NTFS file system.

### Share Permissions

| Permission   | Description                                                                                                      |
| ------------ | ---------------------------------------------------------------------------------------------------------------- |
| Full Control | Perform all actions granted by Change and Read permissions, and modify NTFS permissions for files and subfolders |
| Change       | Read, edit, delete, and add files and subfolders                                                                 |
| Read         | View file and subfolder contents                                                                                 |

### NTFS Basic Permissions

| Permission           | Description                                                            |
| -------------------- | ---------------------------------------------------------------------- |
| Full Control         | Add, edit, move, delete files and folders, and change NTFS permissions |
| Modify               | View and modify files and folders, including adding or deleting files  |
| Read & Execute       | Read file contents and execute programs                                |
| List folder contents | View files and subfolders listed within a folder                       |
| Read                 | Read file contents                                                     |
| Write                | Write changes to a file and add new files to a folder                  |
| Special Permissions  | Various advanced permission options                                    |

### NTFS Special Permissions

| Permission                     | Description                                                                                                     |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| Full control                   | Add, edit, move, delete files and folders, and change NTFS permissions                                          |
| Traverse folder / execute file | Access subfolders within a directory structure, even if access to contents at the parent folder level is denied |
| List folder/read data          | View files and folders within the parent folder, and open and view files                                        |
| Read attributes                | View basic attributes of a file or folder                                                                       |
| Read extended attributes       | View extended attributes of a file or folder                                                                    |
| Create files/write data        | Create files within a folder and make changes to a file                                                         |
| Create folders/append data     | Create subfolders within a folder, and add data to files but cannot overwrite existing content                  |
| Write attributes               | Change file attributes                                                                                          |
| Write extended attributes      | Change extended attributes on a file or folder                                                                  |
| Delete subfolders and files    | Delete subfolders and files, without deleting parent folders                                                    |
| Delete                         | Delete parent folders, subfolders, and files                                                                    |
| Read permissions               | Read permissions of a folder                                                                                    |
| Change permissions             | Change permissions of a file or folder                                                                          |
| Take ownership                 | Take ownership of a file or folder, granting full permissions to change any permissions                         |

NTFS permissions apply to the system where the files and folders are hosted, while share permissions are enforced when accessing the shared resource over the network via SMB. NTFS permissions offer administrators more granular control over user actions within files and folders.

### Creating a Network Share

To comprehend SMB and its relation to NTFS, let's create a network share on a Windows 10 target box.

1. **Creating the Folder**: Begin by creating a new folder on the Windows 10 desktop.
2. **Making the Folder a Share**: Utilize the Advanced Sharing option to configure the share, setting permissions accordingly.
3. **Mounting to the Share**: Mount the share to a desired location using appropriate credentials.

Remember, the NTFS permissions on the shared folder will interact with share permissions, providing additional layers of security.

#### Windows Defender Firewall Considerations

The Windows Defender Firewall may block SMB share access, particularly from non-joined devices or those outside the same workgroup. Proper firewall rules or exceptions should be configured to allow access without compromising security.

### Monitoring Shares and Access

- **`net share` Command**: Display all shared folders on the system.
- **Computer Management Tool**: Explore Shares, Sessions, and Open Files to monitor shared resources.
- **Event Viewer**: Review logs to track actions performed on the system, including share access and modifications.
