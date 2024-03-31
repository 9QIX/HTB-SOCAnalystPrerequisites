# Working with Directories and Files

Now that we can safely navigate via the command line, it is time to master the art of files and directories. This can be a robust topic; we have several ways to accomplish the same tasks with Windows. We will cover a few but keep in mind that there are many other ways to work with files and directories. Let us dive in.

## Directories

What is a directory? In this case, it is an overarching folder structure within the Windows filesystem. Our files are nested within this folder structure, and we can move around utilizing common commands we practiced in the last section, such as `cd` and `dir`.

Revisiting our hallway concept from the last section for a second when thinking about directories, we can break it down like this:

- The Drive itself is a disk, but it is also the root directory. So think about the C: drive as our hotel.
- That hotel has many different floors filled with hallways. This level would include directories like Windows, Users, Program Files, and any other directories created by the operating system or the users.
- These floors have multiple halls. Think of each hall as a folder nested with our previous directories. So for the case of Users, we would then have a folder for each user logged into the host. At this point, we are several levels deep into the filesystem. (`C:\Users\htb`) as an example of a directory.
- This continues with other hallways (directories) as the use of the host expands and more software is installed.
- Eventually, we find the room we were looking for and peek in. Think of the door as a file within the directory hive.

### Viewing & Listing Directories

As we said in the previous section, we can issue the `'cd'` command when trying to see what directory we currently reside in. To get a listing of what files are within a directory, we can use the `dir` command, and `tree` provides a complete listing of all files and folders within the specified path. So it is nice to see we already have a head start.

![dir, chdir, tree commands](image.png)

The image above shows how the three can be used in conjunction. `chdir` can also change our current working directory.

### Create A New Directory

Creating a directory to add to our structure is a simple endeavor. We can utilize the `md` and `mkdir` commands.

#### Using MD

```cmd
C:\Users\htb\Desktop> dir

 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop

06/15/2021  09:28 PM    <DIR>          .
06/15/2021  09:28 PM    <DIR>          ..
06/14/2021  10:37 PM                19 file.txt
06/15/2021  09:32 PM    <DIR>          Git-Pulls
06/14/2021  10:59 PM                26 normal-file.txt
06/15/2021  09:29 PM    <DIR>          Notes
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
06/15/2021  09:29 PM    <DIR>          Work-Policies
               5 File(s)            353 bytes
               5 Dir(s)  38,644,342,784 bytes free

C:\Users\htb\Desktop>md new-directory

C:\Users\htb\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop

06/15/2021  10:26 PM    <DIR>          .
06/15/2021  10:26 PM    <DIR>          ..
06/14/2021  10:37 PM                19 file.txt
06/15/2021  09:32 PM    <DIR>          Git-Pulls
06/15/2021  10:26 PM    <DIR>          new-directory
06/14/2021  10:59 PM                26 normal-file.txt
06/15/2021  09:29 PM    <DIR>          Notes
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
06/15/2021  09:29 PM    <DIR>          Work-Policies
               5 File(s)            353 bytes
               6 Dir(s)  38,644,277,248 bytes free
```

Above, `md` is in use. In the next shell, we will see `mkdir` used similarly. Both accomplish the same goal, so use either as you wish.

#### Using `mkdir` to Create Directories.

```cmd
C:\Users\htb\Desktop> mkdir yet-another-dir

C:\Users\htb\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop

06/15/2021  10:28 PM    <DIR>          .
06/15/2021  10:28 PM    <DIR>          ..
06/14/2021  10:37 PM                19 file.txt
06/15/2021  09:32 PM    <DIR>          Git-Pulls
06/15/2021  10:26 PM    <DIR>          new-directory
06/14/2021  10:59 PM                26 normal-file.txt
06/15/2021  09:29 PM    <DIR>          Notes
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
06/15/2021  09:29 PM    <DIR>          Work-Policies
06/15/2021  10:28 PM    <DIR>          yet-another-dir
               5 File(s)            353 bytes
               7 Dir(s)  38,644,056,064 bytes free
```

### Delete Directories

Deleting directories can be accomplished using the `rd` or `rmdir` commands. The commands `rd` and `rmdir` are explicitly meant for removing directory trees and do not deal with specific files or attributes.

Let us look at `rd` and `rmdir` now.

#### RD & RMDIR

```cmd
C:\Users\htb\Desktop> dir

 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop

06/15/2021  10:28 PM    <DIR>          .
06/15/2021  10:28 PM    <DIR>          ..
06/14/2021  10:37 PM                19 file.txt
06/15/2021  09:32 PM    <DIR>          Git-Pulls
06/15/2021  10:26 PM    <DIR>          new-directory
06/14/2021  10:59 PM                26 normal-file.txt
06/15/2021  09:29 PM    <DIR>          Notes
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
06/15/2021  09:29 PM    <DIR>          Work-Policies
06/15/2021  10:28 PM    <DIR>          yet-another-dir
               5 File(s)            353 bytes
               7 Dir(s)  38,634,733,568 bytes free

C:\Users\htb\Desktop> rd Git-Pulls
The directory is not empty.
```

```cmd
RD /S
C:\Users\htb\Desktop> rd /S Git-Pulls
Git-Pulls, Are you sure (Y/N)? Y

C:\Users\htb\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop

06/16/2021  01:32 PM    <DIR>          .
06/16/2021  01:32 PM    <DIR>          ..
06/15/2021  10:26 PM    <DIR>          new-directory
06/14/2021  10:59 PM                26 normal-file.txt
06/15/2021  09:29 PM    <DIR>          Notes
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
06/15/2021  09:29 PM    <DIR>          Work-Policies
06/15/2021  10:28 PM    <DIR>          yet-another-dir
               5 File(s)            353 bytes
               6 Dir(s)  38,634,733,568 bytes free
```

In the session above, we listed the directory to see its contents, then issued the `rd Git-Pulls` command. From the first session window, we can see that it did not execute the command since the directory was not empty. `Rd` has a switch `/S` that we can utilize to erase the directory and its contents. Since we want to make `Git-Pulls` disappear, we will issue it in the second cmd session seen above. The commands we have issued with `rd` are the same as `rmdir`.

Removing directories is pretty simple. If you get stuck trying to remove a directory and are getting a warning saying the directory is not empty, do not forget about the `/S` switch.

### Modifying

Modifying a directory is more complicated than changing a file. The directory holds data within it for other files or directories. We have several options in any case. `Move`, `Robocopy`, and `xcopy` can copy and make changes to directories and their structures.

To use `move`, we have to issue the syntax in this order. When moving directories, it will take the directory and any files within and move it from the source to the destination path specified.

#### Move a Directory

```cmd
C:\Users\htb\Desktop> tree example /F

Folder PATH listing
Volume serial number is 00000032 DAE9:5896
C:\USERS\HTB\DESKTOP\EXAMPLE
│   file-1 - Copy.txt
│   file-1.txt
│   file-2.txt
│   file-3.txt
│   file-5.txt
│   ‎file-4.txt
│
└───more stuff

C:\Users\htb\Desktop> move example C:\Users\htb\Documents\example

        1 dir(s) moved.
```

We ran the `tree` command to see what resided in the `example` directory before copying it. After that, executing `move example C:\Users\htb\Documents\example` placed the `example` directory and all its files into the user's Documents folder. We can validate this by running a `dir` on Documents to see if the directory exists.

#### Validate the Move

```cmd
C:\Users\htb\Desktop> dir C:\Users\htb\Documents

Volume in drive C has no label.
 Volume Serial Number is DAE9-5896

 Directory of C:\Users\htb\Documents

06/17/2021  03:14 PM    <DIR>          .
06/17/2021  03:14 PM    <DIR>          ..
06/17/2021  02:23 PM    <DIR>          example
06/17/2021  02:01 PM    <DIR>          test
04/13/2021  12:21 PM    <DIR>          WindowsPowerShell
04/22/2021  01:11 PM           933,003 Wireshark-lab-2.pcap
               1 File(s)        933,003 bytes
               5 Dir(s)  36,644,110,336 bytes free
```

Moreover, there we have it. The directory `example` exists now within the Documents directory. The following two options have more capability in the ways they can interact with files and directories. We will take a minute to look at `xcopy` since it still exists in current Windows operating systems, but it is essential to know that it has been deprecated for `robocopy`. Where `xcopy` shines is that it can remove the `Read-only` bit from files when moving them. The syntax for `xcopy` is `xcopy source destination options`. As it was with `move`, we can use wildcards for source files, not destination files.

#### Using Xcopy

```cmd
C:\Users\htb\Desktop> xcopy C:\Users\htb\Documents\example C:\Users\htb\Desktop\ /E

C:\Users\htb\Documents\example\file-1 - Copy.txt
C:\Users\htb\Documents\example\file-1.txt
C:\Users\htb\Documents\example\file-2.txt
C:\Users\htb\Documents\example\file-3.txt
C:\Users\htb\Documents\example\file-5.txt
C:\Users\htb\Documents\example\‎file-4.txt
6 File(s) copied
```

Xcopy prompts us during the process and displays the result. In our case, the directory and any files within were copied to the Desktop. Utilizing the `/E` switch, we told Xcopy to copy any files and subdirectories to include empty directories. Keep in mind this will not delete the copy in the previous directory. When performing the duplication, xcopy will reset any attributes the file had. If you wish to retain the file's attributes ( such as read-only or hidden ), you can use the `/K` switch.

From a hacker's perspective, `xcopy` can be extremely helpful. If we wish to move a file, even a system file, or something locked, `xcopy` can do this without adding other tools to the host. As a defender, this is a great way to grab a copy of a file and retain the same state for analysis. For example, you wish to grab a read-only file that was transferred in from a CD or flash drive, and you now suspect it of performing suspicious actions.

`Robocopy` is `xcopy`'s successor built with much more capability. We can think of `Robocopy` as merging the best parts of `copy`, `xcopy`, and `move` spiced up with a few extra capabilities. `Robocopy` can copy and move files locally, to different drives, and even across a network while retaining the file data and attributes to include timestamps, ownership, ACLs, and any flags set like hidden or read-only. We need to be aware that `Robocopy` was made for large directories and drive syncing, so it does not like to copy or move singular files by default. That is not to say it is incapable, however. We will cover a bit of that down below.

#### Robocopy Basic

```cmd
C:\Users\htb\Desktop> robocopy C:\Users\htb\Desktop C:\Users\htb\Documents\

robocopy C:\Users\htb\Desktop C:\Users\htb\Documents

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Monday, June 21, 2021 11:05:46 AM
   Source : C:\Users\htb\Desktop\
     Dest : C:\Users\htb\Documents\

    Files : *.*

  Options : *.* /DCOPY:DA /COPY:DAT /R:1000000 /W:30

------------------------------------------------------------------------------

                           7    C:\Users\htb\Desktop\
        *EXTRA Dir        -1    C:\Users\htb\Documents\My Music\
        *EXTRA Dir        -1    C:\Users\htb\Documents\My Pictures\
        *EXTRA Dir        -1    C:\Users\htb\Documents\My Videos\
100%        Older                    282        desktop.ini
100%        New File                  19        file.txt
100%        New File                  26        normal-file.txt
100%        New File                  97        passwords.txt
100%        New File                  97        Project plans.txt
100%        New File                 114        secrets.txt
100%        New File               38380        Windows Startup.wav

------------------------------------------------------------------------------

               Total    Copied   Skipped  Mismatch    FAILED    Extras
    Dirs :         1         0         1         0         0         3
   Files :         7         7         0         0         0         0
   Bytes :    38.1 k    38.1 k         0         0         0         0
   Times :   0:00:00   0:00:00

   Speed :              619285 Bytes/sec.
   Speed :              35.435 MegaBytes/min.
   Ended : Monday, June 21, 2021 11:05:46 AM


C:\Users\htb\Desktop>dir C:\Users\htb\Documents
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Documents

06/21/2021  11:05 AM    <DIR>          .
06/21/2021  11:05 AM    <DIR>          ..
06/14/2021  10:37 PM                19 file.txt
06/14/2021  10:59 PM                26 normal-file.txt
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
12/07/2019  05:08 AM            38,380 Windows Startup.wav
               6 File(s)         38,733 bytes
               2 Dir(s)  38,285,684,736 bytes free
```

`Robocopy` took everything in our Desktop directory and made a copy of it in the Documents directory. This works without any issues because we have permission over the folder we are trying to copy currently. As discussed earlier, `Robocopy` can also work with system, read-only, and hidden files. As a user, this can be problematic if we do not have the `SeBackupPrivilege` and auditing privilege attributes. This could stop us from duplicating or moving files and directories. There is a bit of a workaround, however. We can utilize the `/MIR` switch to permit ourselves to copy the files we need temporarily.

#### Robocopy Backup Mode Fail

```cmd
C:\Users\htb\Desktop> robocopy /E /B /L C:\Users\htb\Desktop\example C:\Users\htb\Documents\Backup\

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Monday, June 21, 2021 10:03:56 PM
   Source : C:\Users\htb\Desktop\example\
     Dest : C:\Users\htb\Documents\Backup\

    Files : *.*

  Options : *.* /L /S /E /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

ERROR : You do not have the Backup and Restore Files user rights.
*****  You need these to perform Backup copies (/B or /ZB).

ERROR : Robocopy ran out of memory, exiting.
ERROR : Invalid Parameter #%d : "%s"

ERROR : Invalid Job File, Line #%d :"%s"


  Started : %s %s

   Source %c

     Dest %c
       Simple Usage :: ROBOCOPY source destination /MIR

             source :: Source Directory (drive:\path or \\server\share\path).
        destination :: Destination Dir  (drive:\path or \\server\share\path).
               /MIR :: Mirror a complete directory tree.

    For more usage information run ROBOCOPY /?


****  /MIR can DELETE files as well as copy them !
```

From the output above, we can see that our permissions are insufficient. Utilizing the `/MIR` switch will complete the task for us. Be aware that it will mark the files as a system backup and hide them from view. We can clear the additional attributes if we add the `/A-:SH` switch to our command. Be careful of the `/MIR` switch, as it will mirror the destination directory to the source. Any file that exists within the destination will be removed. Ensure you place the new copy in a cleared folder. Above, we also used the `/L` switch. This is a what-if command. It will process the command you issue but not execute it; it just shows you the potential result. Let us give it a try below.

#### Robocopy /MIR

```cmd
C:\Users\htb\Desktop> robocopy /E /MIR /A-:SH C:\Users\htb\Desktop\notes\ C:\Users\htb\Documents\Backup\Files-to-exfil\

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Monday, June 21, 2021 10:45:46 PM
   Source : C:\Users\htb\Desktop\notes\
     Dest : C:\Users\htb\Documents\Backup\Files-to-exfil\

    Files : *.*

  Options : *.* /S /E /DCOPY:DA /COPY:DAT /PURGE /MIR /A-:SH /R:1000000 /W:30

------------------------------------------------------------------------------

                           2    C:\Users\htb\Desktop\notes\
100%        New File                  16        python-notes
100%        New File                  13        vscode

------------------------------------------------------------------------------

               Total    Copied   Skipped  Mismatch    FAILED    Extras
    Dirs :         1         0         1         0         0         0
   Files :         2         2         0         0         0         0
   Bytes :        29        29         0         0         0         0
   Times :   0:00:00   0:00:00                       0:00:00   0:00:00
   Ended : Monday, June 21, 2021 10:45:46 PM


C:\Users\htb\Documents\Backup\Files-to-exfil>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Documents\Backup\Files-to-exfil

06/21/2021  10:45 PM    <DIR>          .
06/21/2021  10:45 PM    <DIR>          ..
06/15/2021  09:29 PM                16 python-notes
06/15/2021  09:28 PM                13 vscode
               2 File(s)             29 bytes
               2 Dir(s)  38,285,676,544 bytes free
```

Running our command and then checking the directory shows us that the files copied over successfully. There are so many ways we can utilize `Robocopy` that it needs its own section. Experiment and play with the tool to develop some of your ways to move directories, copy files, and even play with attributes.

## Files

Many of the same commands we utilized while administering directories can also be used with files. Windows has plenty more built-in tools we can use for all our file magic fun. We will cover a few of them here. We should first discuss how to view files and their contents.

### List Files & View Their Contents

We already know we can utilize the `dir` command to view the files within a directory, along with specific information about them, depending on the switches we use. It is often the easiest way to see what files exist within a directory. We also have the `tree /F` command to show us an output containing all directories and files within the tree. Nevertheless, what if we wish to view the contents of a file? We can utilize the `more`, `openfiles`, and `type` commands.

First up is `more`. With this built-in tool, we can view the contents of a file or the results of another command printed to it one screen at a time. Think of it as a way to buffer scrolling text that may otherwise overflow the terminal buffer.

#### More

```cmd
C:\Users\htb\Documents\Backup> more secrets.txt

The TVA has several copies of the Infinity Stones..


Bucky is a good guy. TWS is a Bo$$


The sky isn't blue..


-- More (6%) --
```

Notice that the bottom of the cmd-session shows us the percentage of the file being viewed. as we hit enter or the space bar, it will scroll the document's text for us, showing an increasing amount of the file in view. With large files containing multiple blank lines or a large amount of empty space between data, we can use the `/S` option to crunch that blank space down to a single line at each point to make it easier to view. This will not modify the file, just like the `more` command outputs blank space.

#### More /S

```cmd
C:\Users\htb\Documents\Backup> more /S secrets.txt

The TVA has several copies of the Infinity Stones..

Bucky is a good guy. TWS is a Bo$$

The sky isn't blue..

Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-LSM3BSF
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : lan

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : lan
   Description . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-0C-29-D7-67-BF
-- More (27%) --
```

Notice how we have much more of the file in our first window view. `More` took a large amount of blank space and compressed it.

#### Sending a Command Output to More

```
C:\Users\htb\> ipconfig /all | more

Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-LSM3BSF
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : lan

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : lan
   Description . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-0C-29-D7-67-BF
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::59fe:9ed2:fea6:1371%5(Preferred)
   IPv4 Address. . . . . . . . . . . : 172.16.146.5(Preferred)
-- More  --
```

In the output above, we issued the `ipconfig /all` command which generally outputs a bunch of data, and piped (`|`) through `more` to slow it down. This is especially handy when dealing with large files or commands that generate a lot of text, such as `systeminfo`.

With `openfiles`, we can see what file on our local pc or a remote host has open and from which user. This command requires administrator privileges on the host you are trying to view. With this tool, we can view open files, disconnect open files, and even kick users from accessing specific files. The ability to use this command is not enabled by default on Windows systems.

`Type` can display the contents of multiple text files at once. It is also possible to utilize fire redirection with `type` as well. It is a simple tool but extremely handy. One interesting thing about `type` is that it will not lock files, so there is no worry of messing something up.

#### Type

```cmd
C:\Users\htb\Desktop>type bio.txt

James Buchanan "Bucky" Barnes Jr. is a fictional character appearing in American comic books published by Marvel Comics. Originally introduced as a sidekick to Captain America, the character was created by Joe Simon and Jack Kirby and first appeared in Captain America Comics #1 (cover-dated March 1941) (which was published by Marvel's predecessor, Timely Comics). Barnes' original costume (or one based on it) and the Bucky nickname have been used by other superheroes in the Marvel Universe over the years.[1] The character is brought back from supposed death as the brainwashed assassin cyborg called Winter Soldier (Russian: ╨ù╨╕╨╝╨╜╨╕╨╣ ╨í╨╛╨╗╨┤╨░╤é, translit. Zimniy Sold├ít). The character's memories and personality are later restored, leading him to become a dark hero in search of redemption. He temporarily assumes the role of "Captain America" when Steve Rogers was presumed to be dead. During the 2011 crossover Fear Itself, Barnes is injected with the Infinity Formula, which increases his natural vitality and physical traits in a way that is similar to (but less powerful than) the super-soldier serum used on Captain America.[2]
```

That is all there is to it. `Type` provides Simple file output. We can also use it to send output to another file. This can be a quick way to write a new file or append data to another file.

#### Redirect With Type

```cmd
C:\Users\htb\Desktop>type passwords.txt >> secrets.txt

C:\Users\htb\Desktop>type secrets.txt

The TVA has several copies of the Infinity Stones..
Bucky is a good guy. TWS is a Bo$$
The sky isn't blue..
" so many passwords in the file.. "
Password P@ssw0rd Super$ecr3t Admin @dmin123 Summer2021!
```

With the example above, we appended the `passwords.txt` file to the end of the `secrets.txt` file with `>>`. Then we viewed the contents of `secrets.txt` and can see our data was successfully added.

We have been discussing a relatively simple topic, but it is a crucial part of any administrator or hacker's job. Utilizing built-in tools such as `type` and `more` to poke around in a host filesystem is a quick and reasonably unnoticeable way to look for passwords, company rosters, or other potentially sensitive information.

### Create And Modify A File

Creating and modifying a file from the command line is relatively easy. We have several options that include `echo`, `fsutil`, `ren`, `rename`, and `replace`. First, `echo` with output redirection allows us to modify a file if it already exists or create a new file at the time of the call.

#### Echo to Create and Append Files

```cmd
C:\Users\htb\Desktop>echo Check out this text > demo.txt

C:\Users\htb\Desktop>type demo.txt
Check out this text

C:\Users\htb\Desktop>echo More text for our demo file >> demo.txt

C:\Users\htb\Desktop>type demo.txt
Check out this text
More text for our demo file
```

With `fsutil`, we can do many things, but in this instance, we will use it to create a file.

#### Fsutil to Create a file

```
C:\Users\htb\Desktop>fsutil file createNew for-sure.txt 222
File C:\Users\htb\Desktop\for-sure.txt is created

C:\Users\htb\Desktop>echo " my super cool text file from fsutil "> for-sure.txt

C:\Users\htb\Desktop>type for-sure.txt
" my super cool text file from fsutil "
```

`Ren` allows us to change the name of a file to something new.

#### Ren(ame) A file

```cmd
C:\Users\htb\Desktop> ren demo.txt superdemo.txt

C:\Users\htb\Desktop>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop

06/22/2021  04:25 PM    <DIR>          .
06/22/2021  04:25 PM    <DIR>          ..
06/22/2021  03:21 PM             1,140 bio.txt
06/16/2021  02:36 PM    <DIR>          example
06/14/2021  10:37 PM                19 file.txt
06/22/2021  04:12 PM                41 for-sure.txt
06/22/2021  03:59 PM                12 maybe.txt
06/15/2021  10:26 PM    <DIR>          new-directory
06/22/2021  03:48 PM                 9 nono.txt
06/14/2021  10:59 PM                26 normal-file.txt
06/15/2021  09:29 PM    <DIR>          Notes
06/14/2021  10:28 PM                97 passwords.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/22/2021  03:24 PM               211 secrets.txt
06/22/2021  04:14 PM                52 superdemo.txt
06/22/2021  03:18 PM             2,534 type.txt
06/21/2021  11:33 AM                 0 why-tho.txt
12/07/2019  05:08 AM            38,380 Windows Startup.wav
06/15/2021  09:29 PM    <DIR>          Work-Policies
06/15/2021  10:28 PM    <DIR>          yet-another-dir
              13 File(s)         42,618 bytes
               7 Dir(s)  39,091,531,776 bytes free
```

We utilized ren to change the name of demo.txt to superdemo.txt. It can be issued as ren or rename. They are links to the same basic command.

### Input / Output

We have seen this a few times already, but let us take a minute to talk about I/O. We can utilize the <, >, |, and & to send input and output from the console and files to where we need them. With > we can push the output of a command to a file.

## Output To A File

```cmd
Working with Directories and Files
C:\Users\htb\Documents>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Documents

06/23/2021  02:44 PM    <DIR>          .
06/23/2021  02:44 PM    <DIR>          ..
06/21/2021  10:38 PM    <DIR>          Backup
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
               2 File(s)            211 bytes
               3 Dir(s)  39,028,850,688 bytes free

C:\Users\htb\Documents>ipconfig /all > details.txt

C:\Users\htb\Documents>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Documents

06/23/2021  02:44 PM    <DIR>          .
06/23/2021  02:44 PM    <DIR>          ..
06/21/2021  10:38 PM    <DIR>          Backup
06/23/2021  02:44 PM             1,813 details.txt
06/14/2021  10:34 PM                97 Project plans.txt
06/14/2021  08:38 PM               114 secrets.txt
               3 File(s)          2,024 bytes
               3 Dir(s)  39,028,760,576 bytes free

C:\Users\htb\Documents>type details.txt

Windows IP Configuration

   Host Name . . . . . . . . . . . . : DESKTOP-LSM3BSF
   Primary Dns Suffix  . . . . . . . :
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : greenhorn.corp

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : greenhorn.corp
   Description . . . . . . . . . . . : Intel(R) 82574L Gigabit Network Connection
   Physical Address. . . . . . . . . : 00-0C-29-D7-67-BF
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::59fe:9ed2:fea6:1371%8(Preferred)
   IPv4 Address. . . . . . . . . . . : 172.16.146.5(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Lease Obtained. . . . . . . . . . : Wednesday, June 23, 2021 2:42:19 PM
   Lease Expires . . . . . . . . . . : Thursday, June 24, 2021 2:27:59 PM
   Default Gateway . . . . . . . . . : 172.16.146.1
```

Looking above, we can see that the output from our ipconfig /all command was pushed to details.txt. When we check the file, we see when it was created, and the content's output successfully inside. Using > this way will create the file if it does not exist, or it will overwrite the specified file's contents. To append to an already populated file, we can utilize >>.

## Append to a File

```cmd
Working with Directories and Files
C:\Users\htb\Documents> echo a b c d e > test.txt

C:\Users\htb\Documents>type test.txt
a b c d e

C:\Users\htb\Documents>echo f g h i j k see how this works now? >> test.txt

C:\Users\htb\Documents>type test.txt
a b c d e
f g h i j k see how this works now?
```

We created the test.txt file with a string, then appended our following line (f g h i j k see how this works now?) to the file with >>. We were feeding input from a command out before; let us feed input into a command now. We will accomplish that with <.

## Pass in a Text File to a Command

```cmd
Working with Directories and Files
C:\Users\htb\Documents>find /i "see" < test.txt

f g h i j k see how this works now?
```

In the session above, we took the contents of test.txt and fed it into our find command. In this way, we were searching for the string see. We can see it kicked back the results by showing us the line where it found see. These were fairly simple commands, but remember that we can use < like this to search for keywords or strings in large text files, sort for unique items, and much more. This can be extremely helpful for us as a hacker looking for key information. Another route we can take is to feed the output from a command directly into another command with the | called pipe.

## Pipe Output Between Commands

```cmd
Working with Directories and Files
C:\Users\htb\Documents>ipconfig /all | find /i "IPV4"

   IPv4 Address. . . . . . . . . . . : 172.16.146.5(Preferred)
```

With pipe, we could issue the command ipconfig /all and send it to find to search for a specific string. We know it worked because it returns our result in the following line. This effectively took our console output and redirected it to a new pipe. If you get this concept down, you can do endless things.

Let us say we wish to have two commands executed in succession. We can issue the command and follow it with & and then our next command. This will ensure that in this instance, our command A runs first then the session will run command B. It does not care if the command succeeded or failed. It just issues them.

## Run A Then B

```cmd
Working with Directories and Files
C:\Users\htb\Documents>ping 8.8.8.8 & type test.txt

Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=22ms TTL=114
Reply from 8.8.8.8: bytes=32 time=19ms TTL=114
Reply from 8.8.8.8: bytes=32 time=17ms TTL=114
Reply from 8.8.8.8: bytes=32 time=16ms TTL=114

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 16ms, Maximum = 22ms, Average = 18ms
a b c d e
f g h i j k see how this works now?
```

If we care about the result or state of the commands being run, we can utilize && to say run command A, and if it succeeds, run command B. This can be useful if you are doing something that is results dependent such as our cmd-session below.

## State Dependent &&

```cmd
Working with Directories and Files
C:\Users\student\Documents>cd C:\Users\student\Documents\Backup && echo 'did this work' > yes.txt

C:\Users\student\Documents\Backup>type yes.txt
'did this work'
```

We can see that on my first line with &&, we asked to change our working directory, then echo a string into a file if it succeeded. We can tell it succeeded because our cmd path changed and when we type the file, it echo'd our string into the file. You can also accomplish the opposite of this with ||. By using (pipe pipe), we are saying run command A. If it fails, run command B.

We have spent much time leveling up our file creation and modification skills. Now, what if we want to remove objects from the host? Let us look at the del and erase commands.

## Deleting Files

### Dynamic Del And Erase

```cmd
Working with Directories and Files
C:\Users\htb\Desktop\example> dir

 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop\example

06/16/2021  02:00 PM    <DIR>          .
06/16/2021  02:00 PM    <DIR>          ..
06/16/2021  02:00 PM                 5 file-1
06/16/2021  02:00 PM                 5 file-2
06/16/2021  02:00 PM                 5 file-3
06/16/2021  02:00 PM                 5 file-4
06/16/2021  02:00 PM                 5 file-5
06/16/2021  02:00 PM                 5 file-6
06/16/2021  02:00 PM                 5 file-66
               7 File(s)             35 bytes
               2 Dir(s)  38,633,730,048 bytes free

C:\Users\htb\Desktop\example>del file-1

C:\Users\htb\Desktop\example>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop\example

06/16/2021  02:03 PM    <DIR>          .
06/16/2021  02:03 PM    <DIR>          ..
06/16/2021  02:00 PM                 5 file-2
06/16/2021  02:00 PM                 5 file-3
06/16/2021  02:00 PM                 5 file-4
06/16/2021  02:00 PM                 5 file-5
06/16/2021  02:00 PM                 5 file-6
06/16/2021  02:00 PM                 5 file-66
               6 File(s)             30 bytes
               2 Dir(s)  38,633,730,048 bytes free
```

When utilizing del or erase, remember that we can specify a directory, a filename, a list of names, or even a specific attribute to target when trying to delete files. Above, we listed the example directory and then deleted file-1. Simple enough, right? Now let us erase a list of files.

### Using Del And Erase to remove a list of files

```cmd
Working with Directories and Files
C:\Users\htb\Desktop\example> erase file-3 file-5

dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop\example

06/16/2021  02:06 PM    <DIR>          .
06/16/2021  02:06 PM    <DIR>          ..
06/16/2021  02:00 PM                 5 file-2
06/16/2021  02:00 PM                 5 file-4
06/16/2021  02:00 PM                 5 file-6
06/16/2021  02:00 PM                 5 file-66
               4 File(s)             20 bytes
               2 Dir(s)  38,633,218,048 bytes free
```

We can see in the session above that we utilized erase instead of del this time. This was to show the interoperability of both commands. Think of them as symbolic links. Both commands do the same thing. This time we fed erase a list of two files, file-3 and file-5. It erased the files without issue.

Let us say we want to get rid of a read-only or hidden file. We can do that with the /A: switch. /A can delete files based on a specific attribute. Let us look at the help for del quickly and see what those attributes are.

### Del Help Documentation

```cmd
Working with Directories and Files
C:\Users\htb\Desktop\example> help del

Deletes one or more files.

DEL [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names
ERASE [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names

  names         Specifies a list of one or more files or directories.
                Wildcards may be used to delete multiple files. If a
                directory is specified, all files within the directory
                will be deleted.

  /P            Prompts for confirmation before deleting each file.
  /F            Force deleting of read-only files.
  /S            Delete specified files from all subdirectories.
  /Q            Quiet mode, do not ask if ok to delete on global wildcard
  /A            Selects files to delete based on attributes
  attributes    R  Read-only files            S  System files
                H  Hidden files               A  Files ready for archiving
                I  Not content indexed Files  L  Reparse Points
                O  Offline files              -  Prefix meaning not
```

So, to delete a read-only file, we can use A:R. This will remove anything within our path that is Read-only. However, how do we identify if a file is read-only, hidden, or has some other attribute? Dir can come to the rescue again. Utilizing dir /A:R will show us anything with the Read-only attribute. Let us give it a try.

### View Files With the Read-only Attribute

```cmd
Working with Directories and Files
C:\Users\htb\Desktop\example> dir /A:R

 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop\example

06/16/2021  02:00 PM                 5 file-66
               1 File(s)              5 bytes
               0 Dir(s)  38,632,652,800 bytes free
```

Now we know one file matches our Read-only attribute in the example directory. Let us delete it.

### Delete a Read-only File

```cmd
Working with Directories and Files
C:\Users\htb\Desktop\example > del /A:R *

C:\Users\htb\Desktop\example\*, Are you sure (Y/N)? Y

C:\Users\htb\Desktop\example>dir
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop\example

06/16/2021  02:22 PM    <DIR>          .
06/16/2021  02:22 PM    <DIR>          ..
06/16/2021  02:00 PM                 5 file-2
06/16/2021  02:00 PM                 5 file-4
06/16/2021  02:00 PM                 5 file-6
               3 File(s)             15 bytes
               2 Dir(s)  38,632,529,920 bytes free
```

Notice that we used \* to specify any file. Now when we look at the example directory again, file-66 is missing, but files 2, 4, and 6 are still there. Let us give del a swing again with the hidden attribute. To identify if there are any hidden files within the directory, we can use dir /A:H

### Viewing Hidden Files

```cmd
Working with Directories and Files
C:\Users\htb\Desktop\example> dir /A:H
 Volume in drive C has no label.
 Volume Serial Number is 26E7-9EE4

 Directory of C:\Users\htb\Desktop\example

06/16/2021  02:00 PM                 5 file-99
               1 File(s)              5 bytes
               0 Dir(s)  38,632,202,240 bytes free
```

Notice the new file we did not see before? Now file-99 is showing up in our directory listing hidden files. Remember that much like Linux, you can hide files from the view of users. With the hidden attribute, the file exists and can be called, but it will not be visible within a directory listing or from the GUI unless specifically looking for them. To delete the hidden file, we can perform the same del command as earlier, just changing the attribute
