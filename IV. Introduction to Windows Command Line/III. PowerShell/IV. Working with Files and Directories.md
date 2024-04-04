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
```
