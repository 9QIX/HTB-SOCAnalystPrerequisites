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
   ![alt text](/Images/image-15.png)
2. **Making the Folder a Share**: Utilize the Advanced Sharing option to configure the share, setting permissions accordingly.
   ![alt text](/Images/image-16.png)
3. **Mounting to the Share**: Mount the share to a desired location using appropriate credentials.
   ![alt text](/Images/image-18.png)

Remember, the NTFS permissions on the shared folder will interact with share permissions, providing additional layers of security.

#### Using smbclient to Connect to the Share

```ps1
z0x9n@htb[/htb]$ smbclient -L IPaddressOfTarget -U htb-student
Enter WORKGROUP\htb-student's password:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	Company Data    Disk
	IPC$            IPC       Remote IPC
```

#### Windows Defender Firewall Considerations

The Windows Defender Firewall may block SMB share access, particularly from non-joined devices or those outside the same workgroup. Proper firewall rules or exceptions should be configured to allow access without compromising security.

Windows Defender Firewall Profiles:

- Public
- Private
- Domain

### NTFS Permissions ACL (Security Tab)

![alt text](image.png)

The text highlights the significance of NTFS permissions in Windows systems, emphasizing the granular control they offer over users and groups. It explains that gray checkmarks denote permissions inherited from parent directories, with all NTFS permissions being inherited by default. The role of system administrators in managing permissions, particularly over network resources, is underscored, with an indication of their influence compared to non-technical leaders within organizations.

Additionally, it mentions the heightened risk associated with attacks targeting system administrators due to their extensive control over network environments. Finally, it suggests an experiment involving granting the Everyone group Full control at the share level to assess the impact on system behavior.

#### Mounting to the Share

```ps1
z0x9n@htb[/htb]$ sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //ipaddoftarget/"Company Data" /home/user/Desktop/
```

If this command is not working check the syntax. If the syntax is correct yet the command is still not working, cifs-utils may need to be installed. This can be done with the following command:

#### Installing CIFS Utilities

```ps1
z0x9n@htb[/htb]$ sudo apt-get install cifs-utils
```

Once we have successfully created the mount point on the Desktop on our Pwnbox, we should look at a couple of tools built-in to Windows that will allow us to track and monitor what we have done.

The net share command allows us to view all the shared folders on the system. Notice the share we created and also the C:\ drive.

Do you remember us sharing the C:\ drive?

We didn't manually share C:. The most important drive with the most critical files on a Windows system is shared via SMB at install. This means anyone with the proper access could remotely access the entire C:\ of each Windows system on a network.

We can also see the share we created.

#### Displaying Shares using net share

```ps1
C:\Users\htb-student> net share

Share name   Resource                        Remark

-------------------------------------------------------------------------------
C$           C:\                             Default share
IPC$                                         Remote IPC
ADMIN$       C:\WINDOWS                      Remote Admin
Company Data C:\Users\htb-student\Desktop\Company Data

The command completed successfully.
```

Computer Management is another tool we can use to identify and monitor shared resources on a Windows system.

#### Monitoring Shares from Computer Management

![alt text](/Images/image-20.png)

We can poke around in Shares, Sessions, and Open Files to get an idea of what information this provides us. Should there be a situation where we assist an individual or organization with responding to a breach related to SMB, these are some great places to check and start to understand how the breach may have happened and what may have been left behind.

#### Viewing Share access logs in Event Viewer

Event Viewer is another good place to investigate actions completed on Windows. Almost every operating system has a logging mechanism and a utility to view the logs that were captured. Know that a log is like a journal entry for a computer, where the computer writes down all the actions that were performed and numerous details associated with that action. We can view the logs created for every action we performed when accessing the Windows 10 target box, as well as when creating, editing and accessing the shared folder.

![alt text](/Images/image-21.png)
