# Working with Files and Directories

We already know how to navigate around the host and manage users and groups utilizing only PowerShell; now, it is time to explore files and directories. In this section, we will experiment with creating, modifying, and deleting files and directories, along with a quick introduction to file permissions and how to enumerate them. By now, we should be familiar with the Get, Set, New verbs, among others, so we will speed this up with our examples by combining several commands into a single shell session.

## Creating/Moving/Deleting Files & Directories

Many of the cmdlets we will discuss in this section can apply to working with files and folders, so we will combine some of our actions to work more efficiently (as any good pentester or sysadmin should strive to.). The table below lists the commonly used cmdlets used when dealing with objects in PowerShell.

### Common Commands Used for File & Folder Management

| Command        | Alias            | Description                                                                                             |
| -------------- | ---------------- | ------------------------------------------------------------------------------------------------------- |
| Get-Item       | gi               | Retrieve an object (could be a file, folder, registry object, etc.)                                     |
| Get-ChildItem  | ls / dir / gci   | Lists out the content of a folder or registry hive.                                                     |
| New-Item       | md / mkdir / ni  | Create new objects. ( can be files, folders, symlinks, registry entries, and more)                      |
| Set-Item       | si               | Modify the property values of an object.                                                                |
| Copy-Item      | copy / cp / ci   | Make a duplicate of the item.                                                                           |
| Rename-Item    | ren / rni        | Changes the object name.                                                                                |
| Remove-Item    | rm / del / rmdir | Deletes the object.                                                                                     |
| Get-Content    | cat / type       | Displays the content within a file or object.                                                           |
| Add-Content    | ac               | Append content to a file.                                                                               |
| Set-Content    | sc               | overwrite any content in a file with new data.                                                          |
| Clear-Content  | clc              | Clear the content of the files without deleting the file itself.                                        |
| Compare-Object | diff / compare   | Compare two or more objects against each other. This includes the object itself and the content within. |

### Scenario: Greenhorn's new Security Chief, Mr. Tanaka, has requested that a set of files and folders be created for him. He plans to use them for SOP documentation for the Security team. Since he just got host access, we have agreed to set the file & folder structure up for him. If you would like to follow along with the examples below, please feel free. For your practice, we removed the folders and files discussed below so you can take a turn recreating them.

First, we are going to start with the folder structure he requested. We are going to make three folders named :

- SOPs
- Physical Sec
- Cyber Sec
- Training

We will use the Get-Item, Get-ChildItem, and New-Item commands to create our folder structure. Let us get started. We first need to determine where we are in the host and then move to Mr. Tanaka's Documents folder.

#### Finding Our Place

```
Working with Files and Directories
PS C:\htb> Get-Location

Path
----
C:\Users\MTanaka

PS C:\Users\MTanaka> cd Documents
PS C:\Users\MTanaka\Documents>
```

Now that we are in the correct directory, it's time to get to work. Next, we need to make the SOPs folder. The New-Item Cmdlet can be used to accomplish this.

#### New-Item

```
Working with Files and Directories
PS C:\Users\MTanaka\Documents>  new-item -name "SOPs" -type directory


    Directory: C:\Users\MTanaka\Documents


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         10/5/2022  12:20 PM                SOPs
```

Awesome. Our main directory exists now. Let us create our nested folders Physical Sec, Cyber Sec, and Training. We can utilize the same command from last time or the alias mkdir. First, we need to move into the SOPs Directory.

#### Making More Directories

```
Working with Files and Directories
PS C:\Users\MTanaka\Documents> cd SOPs

PS C:\Users\MTanaka\Documents\SOPs> mkdir "Physical Sec"

    Directory: C:\Users\MTanaka\Documents\SOPs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         10/5/2022   4:30 PM                Physical Sec

PS C:\Users\MTanaka\Documents\SOPs> mkdir "Cyber Sec"

    Directory: C:\Users\MTanaka\Documents\SOPs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         10/5/2022   4:30 PM                Cyber Sec

PS C:\Users\MTanaka\Documents\SOPs> mkdir "Training"

    Directory: C:\Users\MTanaka\Documents\SOPs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         10/5/2022   4:31 PM                Training

PS C:\Users\MTanaka\Documents\SOPs> Get-ChildItem

Directory: C:\Users\MTanaka\Documents\SOPs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        10/5/2022   9:08 AM                Cyber Sec
d-----        11/5/2022   9:09 AM                Physical Sec
d-----        11/5/2022   9:08 AM                Training
```

Now that we have our directory structure in place. It's time to start populating the files required. Mr. Tanaka asked for a Markdown file in each folder like so:

- SOPs > ReadMe.md
- Physical Sec > Physical-Sec-draft.md
- Cyber Sec > Cyber-Sec-draft.md
- Training > Employee-Training-draft.md

In each file, he has requested this header at the top:

```
Title: Insert Document Title Here
Date: x/x/202x
Author: MTanaka
Version: 0.1 (Draft)
```

We should be able to quickly knock this out using the New-Item cmdlet and the Add-Content cmdlet.

#### Making Files

```
Working with Files and Directories
PS C:\htb> PS C:\Users\MTanaka\Documents\SOPs> new-Item "Readme.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:12 AM              0 Readme.md

PS C:\Users\MTanaka\Documents\SOPs> cd '.\Physical Sec\'
PS C:\Users\MTanaka\Documents\SOPs\Physical Sec> ls
PS C:\Users\MTanaka\Documents\SOPs\Physical Sec> new-Item "Physical-Sec-draft.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs\Physical Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Physical-Sec-draft.md

PS C:\Users\MTanaka\Documents\SOPs\Physical Sec> cd ..
PS C:\Users\MTanaka\Documents\SOPs> cd '.\Cyber Sec\'

PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> new-Item "Cyber-Sec-draft.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs\Cyber Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Cyber-Sec-draft.md

PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> cd ..
PS C:\Users\MTanaka\Documents\SOPs> cd .\Training\
PS C:\Users\MTanaka\Documents\SOPs\Training> ls
PS C:\Users\MTanaka\Documents\SOPs\Training> new-Item "Employee-Training-draft.md" -ItemType File

    Directory: C:\Users\MTanaka\Documents\SOPs\Training

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/
10/2022   9:15 AM              0 Employee-Training-draft.md

PS C:\Users\MTanaka\Documents\SOPs\Training> cd ..
PS C:\Users\MTanaka\Documents\SOPs> tree /F
Folder PATH listing
Volume serial number is F684-763E
C:.
│   Readme.md
│
├───Cyber Sec
│       Cyber-Sec-draft.md
│
├───Physical Sec
│       Physical-Sec-draft.md
│
└───Training
        Employee-Training-draft.md
```

Now that we have our files, we need to add content inside them. We can do so with the Add-Content cmdlet.

#### Adding Content

```
Working with Files and Directories
PS C:\htb> Add-Content .\Readme.md "Title: Insert Document Title Here
>> Date: x/x/202x
>> Author: MTanaka
>> Version: 0.1 (Draft)"

PS C:\Users\MTanaka\Documents\SOPs> cat .\Readme.md
Title: Insert Document Title Here
Date: x/x/202x
Author: MTanaka
Version: 0.1 (Draft)
```

We would then perform this same process we did for Readme.md in every other file we created for Mr. Tanaka. This scenario felt a bit tedious, right? Creating files over and over by hand can get tiresome. This is where automation and scripting come into place. It is a bit out of reach right now, but in a later section in this module, we will discuss how to make a quick PowerShell Module, using variables and writing scripts to make things easier.

### Scenario Cont.: Mr. Tanaka has asked us to change the name of the file `Cyber-Sec-draft.md` to `Infosec-SOP-draft.md`.

We can quickly knock this task out using the Rename-Item cmdlet. Lets' give it a try:

#### Renaming An Object

```
Working with Files and Directories
PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> ls

    Directory: C:\Users\MTanaka\Documents\SOPs\Cyber Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Cyber-Sec-draft.md

PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> Rename-Item .\Cyber-Sec-draft.md -NewName Infosec-SOP-draft.md
PS C:\Users\MTanaka\Documents\SOPs\Cyber Sec> ls

    Directory: C:\Users\MTanaka\Documents\SOPs\Cyber Sec

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/10/2022   9:14 AM              0 Infosec-SOP-draft.md
```

All we needed to do above was issue the Rename-Item cmdlet, give it the original filename we want to change (Cyber-Sec-draft.md), and then tell it our new name with the -NewName (Infosec-SOP-draft.md) parameter. Seems simple right? We could take this further and rename all files within a directory or change the file type or several different actions. In our example below, we will change the names of all text files in Mr. Tanakas Desktop from file.txt to file.md.

Files1-5.txt are on MTanaka's Desktop

```
Working with Files and Directories
PS C:\Users\MTanaka\Desktop> ls

    Directory: C:\Users\MTanaka\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/13/2022   1:05 PM              0 file-1.txt
-a----        10/13/2022   1:05 PM              0 file-2.txt
-a----        10/13/2022   1:06 PM              0 file-3.txt
-a----        10/13/2022   1:06 PM              0 file-4.txt
-a----        10/13/2022   1:06 PM              0 file-5.txt

PS C:\Users\MTanaka\Desktop> get-childitem -Path *.txt | rename-item -NewName {$_.name -replace ".txt",".md"}
PS C:\Users\MTanaka\Desktop> ls

    Directory: C:\Users\MTanaka\Desktop

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        10/13/2022   1:05 PM              0 file-1.md
-a----        10/13/2022   1:05 PM              0 file-2.md
-a----        10/13/2022   1:06 PM              0 file-3.md
-a----        10/13/2022   1:06 PM              0 file-4.md
-a----        10/13/2022   1:06 PM              0 file-5.md
```

As we can see above, we had five text files on the Desktop. We changed them to .md files using get-childitem -Path \*.txt to select the objects and used | to send those objects to the rename-item -NewName {$_.name -replace ".txt",".md"} cmdlet which renames everything from its original name ($\_.name) and replaces the .txt from name to .md. This is a much faster way to interact with files and perform bulk actions. Now that we have completed all of Mr. Tanakas' requests, let us discuss File and Directory permissions for a second.

## What are File & Directory Permissions

Permissions, simplified, are our host's way of determining who has access to a specific object and what they can do with it. These permissions allow us to apply granular security control over our objects to maintain a proper security posture. In environments like large organizations with multiple departments (like HR, IT, Sales, etc.), want to ensure they keep information access on a "need to know" basis. This ensures that an outsider cannot corrupt or misuse the data. The Windows file system has many basic and advanced permissions. Some of the key permission types are:

### Permission Types Explained

- **Full Control**: Full Control allows for the user or group specified the ability to interact with the file as they see fit. This includes everything below, changing the permissions, and taking ownership of the file.
- **Modify**: Allows reading, writing, and deleting files and folders.
- **List Folder Contents**: This makes viewing and listing folders and subfolders possible along with executing files. This only applies to folders.
- **Read and Execute**: Allows users to view the contents within files and run executables (.ps1, .exe, .bat, etc.)
- **Write**: Write allows a user the ability to create new files and subfolders along with being able to add content to files.
- **Read**: Allows for viewing and listing folders and subfolders and viewing a file's contents.
- **Traverse Folder**: Traverse allows us to give a user the ability to access files or subfolders within a tree but not have access to the higher-level folder's contents. This is a way to provide selective access from a security perspective.

Windows ( NTFS, in general ) allows us to set permissions on a parent directory and have those permissions populate each file and folder located within that directory. This saves us a ton of time compared to manually setting the permissions on each object contained within. This inheritance can be disabled as necessary for specific files, folders, and sub-folders. If done, we will have to set the permissions we want on the affected files manually. Working with permissions can be a complex task and a bit much to do just from the CLI, so we will leave playing with permissions to the Windows Fundamentals Module.

Working with Files and Directories is straightforward, even if sometimes a bit tedious. Moving forward, we will add another layer to our CLI foundation and look at how we can find and filter content within files on the host.
