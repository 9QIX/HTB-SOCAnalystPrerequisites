# System Navigation

So far, most of what we have covered is introductory information to help us get a basic understanding and feel of the Command Prompt. Continuing with that flow, our next goal should be utilizing our Command Prompt to successfully navigate and move around on the system. In this section, we attempt to conquer our surroundings by:

- Listing A Directory
- Finding Our Place on the System
- Moving Around using CD
- Exploring the File System

Additionally, at the end of the section, we will briefly look into certain directories on a Windows host that might seem juicy from the adversary's perspective. Keeping all of that in mind, let us dive right in and explore the system together.

## Listing A Directory

One of the easiest things we can do when initially poking around on a Windows host is to get a listing of the directory we are currently working in. We do that with the `dir` command.

```
  System Navigation
C:\Users\htb\Desktop> dir

 Volume in drive C has no label.
 Volume Serial Number is DAE9-5896

 Directory of C:\Users\htb\Desktop

06/11/2021  11:59 PM    <DIR>          .
06/11/2021  11:59 PM    <DIR>          ..
06/11/2021  11:57 PM                 0 file1.txt
06/11/2021  11:57 PM                 0 file2.txt
06/11/2021  11:57 PM                 0 file3.txt
04/13/2021  11:24 AM             2,391 Microsoft Teams.lnk
06/11/2021  11:57 PM                 0 super-secret-sauce.txt
06/11/2021  11:59 PM                 0 write-secrets.ps1
               6 File(s)          2,391 bytes
               2 Dir(s)  35,102,117,888 bytes free
```

As seen through the example above, `dir` is an easy-to-use and surprisingly versatile command. Simply calling upon the command without any arguments will give us a listing of our current directory and its contents. As shown in the Getting Help section, we can also use the `/?` argument to provide us with a complete listing of the `dir`'s functionality and any additional arguments that we can provide to utilize it is advanced searching capabilities. In a later section, we will further discuss the meaning behind the above output and how we can use `dir` to aid us in our search for important files and directories. For now, understanding the basic usage of `dir` will provide us with more than enough utility to efficiently move around the system.

## Finding Our Place

Before doing anything on a host, it is helpful to know where we are in the filesystem. We can determine that by utilizing the `cd` or `chdir` commands.

```
  System Navigation
C:\htb> cd

C:\htb
```

As shown by the example above, issuing the command without arguments gives us our current working directory. Our current working directory is our initial starting point. It describes our current directory as the one we are currently working in. Any command(s) run here without specifying the path of another directory or file will reference this initial point. This is very important, considering that everything we do moving forward will reference our current working directory unless specified otherwise.

## Moving Around Using CD/CHDIR

As we were busy finding our place on the system, we introduced the `cd` and `chdir` commands. However, we did not explore the full functionality of either. Besides listing our current directory, both serve an additional function. These commands will move us to whatever directory we specify after the command. The specified directory can either be a directory relative to our current working directory or an absolute directory starting from the filesystem's root.

Those familiar with Linux should begin to recognize this structure and be familiar with the difference between relative paths and absolute paths. However, assuming that we have not come into contact with either of these terms yet, let us quickly showcase the difference using the following examples:

### Current Working Directory

```
  System Navigation
C:\htb> cd

C:\htb
```

This should look familiar, right? It is the same example used in the previous section. Let us expand upon this a bit. First, we need to define our root directory. To keep things simple, think of the root directory as the topmost directory in the structure, as it contains everything else within it. In this example, our root directory is `C:\`.

> Note: `C:\` is the root directory of all Windows machines and has been determined so since it is inception in the MS-DOS and Windows 3.0 days. The "C:\" designation was used commonly as typically "A:\" and "B:\" were recognized as floppy drives, whereas "C:\" was recognized as the first internal hard drive of the machine.

### Absolute Path

```
  System Navigation
C:\htb> cd C:\Users\htb\Pictures

C:\Users\htb\Pictures>
```

In this example, we can see that our initial working directory is located in `C:\htb`. We used `cd` and provided the path as our argument to move ourselves to the `C:\Users\htb\Pictures` directory. As we can see, the provided path starts with `C:\` as it is the root directory and follows the structure until it reaches its destination, being the `\Pictures` directory. Putting the pieces together, we can conclude that `C:\Users\htb\Pictures` would be considered the absolute path in this case as it follows the complete structure of the file system starting from the root directory and ending at the destination directory.

### Relative Path

```
  System Navigation
C:\htb> cd .\Pictures

C:\Users\htb\Pictures>
```

On the other hand, following this example, we can see that something is slightly off in how our path is specified in the `cd` command. Instead of starting from the root directory, we are greeted with a `.` followed by the destination directory (`\Pictures`). The `.` character points to one directory down from our current working directory (`C:\htb`). Using our working directory as the starting point to reference directories either above it or below it in the file system hierarchy is considered a relative path, as its position is relative to the current working directory.

Understanding both of these terms is imperative as we can effectively use this knowledge of the file system's hierarchy to move up and down the file structure with ease. We can piece everything together through one last example to show how quickly we can use what we have learned so far to move about the system.

We are currently in the `C:\Users\htb\Pictures` directory provided in our previous example. However, we wish to quickly move all the way back to the root of the file system in just one command. To do so, we can perform the following:

```
  System Navigation
C:\Users\htb\Pictures>  cd ..\..\..\

C:\>
```

This one command lets us move up the directory structure, starting from the `\Pictures` directory and moving up to the root directory in one swift stroke. Pretty neat, huh? Understanding this fundamental concept will be very important moving forward, so we should practice and familiarize ourselves now while we have the chance.

## Exploring the File System

Using our newfound skills, we should branch out and explore the system earnestly. Thorough exploration is essential, as it can help us gain a considerable advantage in understanding the layout of the system we are interacting with and the files contained within. However, when looking around the filesystem of a Windows host, it can get tedious to change our directory back and forth or to issue the `dir` command for each sub-directory. To save us a bit of time and gain some efficiency, we can get a printout of the entire path we specify and its subdirectories by utilizing the `tree` command.

### Listing the Contents of the File System

```
  System Navigation
C:\htb\student\> tree

Folder PATH listing
Volume serial number is 26E7-9EE4
C:.
├───3D Objects
├───Contacts
├───Desktop
├───Documents
├───Downloads
├───Favorites
│   └───Links
├───Links
├───Music
├───OneDrive
├───Pictures
│   ├───Camera Roll
│   └───Saved Pictures
├───Saved Games
├───Searches
└───Videos
    └───Captures
```

From a hacker perspective, this can be super useful when searching for files and folders with juicy information we may want, like configurations, project files and folders, and maybe even that holy grail, a file or folder containing passwords. We can utilize the `/F` parameter with the `tree` command to see a listing of each file and the directories along with the directory tree of the path.

### Tree /F

```
  System Navigation
C:\htb\student\> tree /F

Folder PATH listing
Volume serial number is 26E7-9EE4
C:.
├───3D Objects
├───Contacts
├───Desktop
│       passwords.txt.txt
│       Project plans.txt
│       secrets.txt
│
├───Documents
├───Downloads
├───Favorites
│   │   Bing.URL
│   │
│   └───Links
├───Links
│       Desktop.lnk
│       Downloads.lnk
│
├───Music
├───OneDrive
├───Pictures
│   ├───Camera Roll
│   └───Saved Pictures
├───Saved Games
├───Searches
│       winrt--{S-1-5-21-1588464669-3682530959-1994202445-1000}-.searchconnector-ms
│
└───Videos
    └───Captures

    <SNIP>
```

From this example, we can quickly get a feel for the system and see some juicy files such as `passwords.txt.txt` and `secrets.txt`. Of course, since this performs a complete listing of every single file and directory on a system, we must be aware of how much output this command will kick up. Later during the module, we will learn a more manageable way of handling the output and working with other command line applications to manipulate it into a much more desirable format. For now, be aware that after attempting to run this command, we should probably interrupt its execution using `Ctrl-C` after retrieving the desired information.

## Interesting Directories

As promised, we have nearly reached the end of this section. With our current skill set, navigating the system should be much more approachable now than initially seemed. Let us take a minute to discuss some directories that can come in handy from an attacker's perspective on a system. Below is a table of common directories that an attacker can abuse to drop files to disk, perform reconnaissance, and help facilitate attack surface mapping on a target host.

| Name                  | Location                             | Description                                                                                                                                                                                                                                                                      |
| --------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `%SYSTEMROOT%\Temp`   | `C:\Windows\Temp`                    | Global directory containing temporary system files accessible to all users on the system. All users, regardless of authority, are provided full read, write, and execute permissions in this directory. Useful for dropping files as a low-privilege user on the system.         |
| `%TEMP%`              | `C:\Users\<user>\AppData\Local\Temp` | Local directory containing a user's temporary files accessible only to the user account that it is attached to. Provides full ownership to the user that owns this folder. Useful when the attacker gains control of a local/domain joined user account.                         |
| `%PUBLIC%`            | `C:\Users\Public`                    | Publicly accessible directory allowing any interactive logon account full access to read, write, modify, execute, etc., files and subfolders within the directory. Alternative to the global Windows Temp Directory as it's less likely to be monitored for suspicious activity. |
| `%ProgramFiles%`      | `C:\Program Files`                   | folder containing all 64-bit applications installed on the system. Useful for seeing what kind of applications are installed on the target system.                                                                                                                               |
| `%ProgramFiles(x86)%` | `C:\Program Files (x86)`             | Folder containing all 32-bit applications installed on the system. Useful for seeing what kind of applications are installed on the target system.                                                                                                                               |

The table provided above is by no means an all-encompassing list of all interesting directories on a Windows host. However, these will likely be targeted as they are useful to attackers.

With the end of this section, we have become proficient at moving around the Windows filesystem and understanding where we are in relation to other directories and files on the system. In the next section, we will discuss gathering system information to provide us with a solid understanding of our surrounding environment.
