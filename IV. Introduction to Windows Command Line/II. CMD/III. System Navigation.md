## System Navigation

So far, most of what we have covered is introductory information to help us get a basic understanding and feel of the Command Prompt. Continuing with that flow, our next goal should be utilizing our Command Prompt to successfully navigate and move around on the system. In this section, we attempt to conquer our surroundings by:

- Listing A Directory
- Finding Our Place on the System
- Moving Around using CD
- Exploring the File System

Additionally, at the end of the section, we will briefly look into certain directories on a Windows host that might seem juicy from the adversary's perspective. Keeping all of that in mind, let us dive right in and explore the system together.

### Listing A Directory

One of the easiest things we can do when initially poking around on a Windows host is to get a listing of the directory we are currently working in. We do that with the `dir` command.

```

System Navigation
C:\Users\htb\Desktop> dir

Volume in drive C has no label.
Volume Serial Number is DAE9-5896

Directory of C:\Users\htb\Desktop

06/11/2021 11:59 PM <DIR> .
06/11/2021 11:59 PM <DIR> ..
06/11/2021 11:57 PM 0 file1.txt
06/11/2021 11:57 PM 0 file2.txt
06/11/2021 11:57 PM 0 file3.txt
04/13/2021 11:24 AM 2,391 Microsoft Teams.lnk
06/11/2021 11:57 PM 0 super-secret-sauce.txt
06/11/2021 11:59 PM 0 write-secrets.ps1
6 File(s) 2,391 bytes
2 Dir(s) 35,102,117,888 bytes free

```

### Finding Our Place

Before doing anything on a host, it is helpful to know where we are in the filesystem. We can determine that by utilizing the `cd` or `chdir` commands.

```

System Navigation
C:\htb> cd

C:\htb

```

### Moving Around Using CD/CHDIR

As we were busy finding our place on the system, we introduced the `cd` and `chdir` commands. However, we did not explore the full functionality of either. Besides listing our current directory, both serve an additional function. These commands will move us to whatever directory we specify after the command. The specified directory can either be a directory relative to our current working directory or an absolute directory starting from the filesystem's root.

### Exploring the File System

Using our newfound skills, we should branch out and explore the system earnestly. Thorough exploration is essential, as it can help us gain a considerable advantage in understanding the layout of the system we are interacting with and the files contained within. However, when looking around the filesystem of a Windows host, it can get tedious to change our directory back and forth or to issue the `dir` command for each sub-directory. To save us a bit of time and gain some efficiency, we can get a printout of the entire path we specify and its subdirectories by utilizing the `tree` command.

### Interesting Directories

As promised, we have nearly reached the end of this section. With our current skill set, navigating the system should be much more approachable now than initially seemed. Let us take a minute to discuss some directories that can come in handy from an attacker's perspective on a system. Below is a table of common directories that an attacker can abuse to drop files to disk, perform reconnaissance, and help facilitate attack surface mapping on a target host.

| Name                | Location                           | Description                                                                                                      |
| ------------------- | ---------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| %SYSTEMROOT%\Temp   | C:\Windows\Temp                    | Global directory containing temporary system files accessible to all users on the system.                        |
| %TEMP%              | C:\Users\<user>\AppData\Local\Temp | Local directory containing a user's temporary files accessible only to the user account that it is attached to.  |
| %PUBLIC%            | C:\Users\Public                    | Publicly accessible directory allowing any interactive logon account full access to files and subfolders within. |
| %ProgramFiles%      | C:\Program Files                   | Folder containing all 64-bit applications installed on the system.                                               |
| %ProgramFiles(x86)% | C:\Program Files (x86)             | Folder containing all 32-bit applications installed on the system.                                               |

With the end of this section, we have become proficient at moving around the Windows filesystem and understanding where we are in relation to other directories and files on the system. In the next section, we will discuss gathering system information to provide us with a solid understanding of our surrounding environment.
