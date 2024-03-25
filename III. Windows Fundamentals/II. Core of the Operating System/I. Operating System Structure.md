# Operating System Structure

In Windows operating systems, the directory structure is crucial for organizing and accessing system files and user data. Let's explore the main directories and their functions:

| Directory                      | Function                                                                                                     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| **Perflogs**                   | Holds Windows performance logs but is typically empty by default.                                            |
| **Program Files**              | On 32-bit systems, hosts all 16-bit and 32-bit programs. On 64-bit systems, only 64-bit programs are stored. |
| **Program Files (x86)**        | Dedicated for 32-bit and 16-bit programs on 64-bit editions of Windows.                                      |
| **ProgramData**                | Contains essential data for certain installed programs to function, accessible by any user.                  |
| **Users**                      | Stores user profiles for each system user, including Default and Public folders.                             |
| **Default**                    | Serves as the default user profile template for new user accounts.                                           |
| **Public**                     | Intended for file sharing among computer users, accessible to all users by default.                          |
| **AppData**                    | Stores per-user application data and settings, including Roaming, Local, and LocalLow subfolders.            |
| **Windows**                    | Houses most of the files required for the Windows operating system to function.                              |
| **System, System32, SysWOW64** | Contains essential DLLs for core Windows features and API functionality.                                     |
| **WinSxS**                     | The Windows Component Store holds copies of all Windows components, updates, and service packs.              |

#### Exploring Directories Using Command Line

We can navigate and explore the file system using various command-line utilities:

- **dir**: Displays directory contents.

  ```ps1
  C:\htb> dir c:\ /a
  Volume in drive C has no label.
  Volume Serial Number is F416-77BE

  Directory of c:\

  08/16/2020 10:33 AM <DIR> $Recycle.Bin
  06/25/2020 06:25 PM <DIR> $WinREAgent
  07/02/2020 12:55 PM 1,024 AMTAG.BIN
  06/25/2020 03:38 PM <JUNCTION> Documents and Settings [C:\Users]
  08/13/2020 06:03 PM 8,192 DumpStack.log
  08/17/2020 12:11 PM 8,192 DumpStack.log.tmp
  08/27/2020 10:42 AM 37,752,373,248 hiberfil.sys
  08/17/2020 12:11 PM 13,421,772,800 pagefile.sys
  12/07/2019 05:14 AM <DIR> PerfLogs
  08/24/2020 10:38 AM <DIR> Program Files
  07/09/2020 06:08 PM <DIR> Program Files (x86)
  08/24/2020 10:41 AM <DIR> ProgramData
  06/25/2020 03:38 PM <DIR> Recovery
  06/25/2020 03:57 PM 2,918 RHDSetup.log
  08/17/2020 12:11 PM 16,777,216 swapfile.sys
  08/26/2020 02:51 PM <DIR> System Volume Information
  08/16/2020 10:33 AM <DIR> Users
  08/17/2020 11:38 PM <DIR> Windows
  7 File(s) 51,190,943,590 bytes
  13 Dir(s) 261,310,697,472 bytes free
  ```

- **tree**: Graphically displays directory structure.
  ```ps1
    C:\htb> tree "c:\Program Files (x86)\VMware"
  Folder PATH listing
  Volume serial number is F416-77BE
  C:\PROGRAM FILES (X86)\VMWARE
  ├───VMware VIX
  │   ├───doc
  │   │   ├───errors
  │   │   ├───features
  │   │   ├───lang
  │   │   │   └───c
  │   │   │       └───functions
  │   │   └───types
  │   ├───samples
  │   └───Workstation-15.0.0
  │       ├───32bit
  │       └───64bit
  └───VMware Workstation
    ├───env
    ├───hostd
    │   ├───coreLocale
    │   │   └───en
    │   ├───docroot
    │   │   ├───client
    │   │   └───sdk
    │   ├───extensions
    │   │   └───hostdiag
    │   │       └───locale
    │   │           └───en
    │   └───vimLocale
    │       └───en
    ├───ico
    ├───messages
    │   ├───ja
    │   └───zh_CN
    ├───OVFTool
    │   ├───env
    │   │   └───en
    │   └───schemas
    │       ├───DMTF
    │       └───vmware
    ├───Resources
    ├───tools-upgraders
    └───x64
  ```

These commands provide insights into the file system layout and help manage files and directories effectively.

```ps1
tree c:\ /f | more
```
