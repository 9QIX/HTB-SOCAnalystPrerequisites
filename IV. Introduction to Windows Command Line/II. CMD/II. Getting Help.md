```markdown
## Getting Help

In the previous section, we were introduced to the general concepts of the Command Prompt and how we can access it. This section will expand upon the previous one by introducing the help functionality within Command Prompt, example output, and some additional resources and concepts.

The Command Prompt has a built-in help function that can provide us with detailed information about the available commands on our system and how to utilize those functions. In this section, we are going to cover the following in greater detail:

- How do we utilize the help functionality within Command Prompt?
- Why utilizing the help functionality is essential?
- Where can we find additional external resources for help?
- How to utilize additional tips and tricks in the Command Prompt?

### How to Get Help

When first looking at the Command Prompt interface, it can be overwhelming to stare at a blank prompt. Some initial questions might emerge, such as:

What commands do I have access to?
How do I use these commands?

Let's work on answering the initial question first. While utilizing the Command Prompt, finding help is as easy as typing `help`. Without any additional parameters, this command provides a list of built-in commands and basic information about each displayed command's usage. Let's take a look at it below.

#### Default Help Usage
```

C:\htb> help

For more information on a specific command, type HELP command-name
ASSOC Displays or modifies file extension associations.
ATTRIB Displays or changes file attributes.
BREAK Sets or clears extended CTRL+C checking.
BCDEDIT Sets properties in boot database to control boot loading.
CACLS Displays or modifies access control lists (ACLs) of files.
CALL Calls one batch program from another.
CD Displays the name of or changes the current directory.
CHCP Displays or sets the active code page number.
CHDIR Displays the name of or changes the current directory.
CHKDSK Checks a disk and displays a status report.
...

```

From this output, we can see that it prints out a list of system commands (built-ins) and provides a basic description of its functionality. This is important because we can quickly and efficiently parse the list of built-in functions provided by the command prompt to find the function that suits our needs. From here, we can transition into answering the second question on how these commands are used. To print out detailed information about a particular command, we can issue the following: `help <command name>`.

#### Help with Commands

```

C:\htb> help time

Displays or sets the system time.

TIME [/T | time]

Type TIME with no parameters to display the current time setting and a prompt
for a new one. Press ENTER to keep the same time.

If Command Extensions are enabled, the TIME command supports
the /T switch which tells the command to just output the
current time, without prompting for a new time.

```

As we can see from the output above, when we issued the command `help time`, it printed the help details for `time`. This will work for any system command built-in but not for every command accessible on the system. Certain commands do not have a help page associated with them. However, they will redirect you to running the proper command to retrieve the desired information. For example, running `help ipconfig` will give us the following output.

#### Detailed Output

```

C:\htb> help ipconfig

This command is not supported by the help utility. Try "ipconfig /?".

```

In the previous example, the help feature let us know that it could not provide more information as the help utility does not directly support it. However, utilizing the suggested command `ipconfig /?` will provide us with the information we need to utilize the command correctly. Be aware that several commands use the `/?` modifier interchangeably with `help`.

### Why Do We Need the Help Utility?

In the last section, we discussed the fundamental aspects of utilizing the help functionality from within the Command Prompt and interpreting some of its output. While understanding the technical details of how to use the help function is important, another fundamental concept here is the following:

Why does the help utility exist, and what use does it serve today when access to the Internet is so prevalent?

This question is multifaceted, so let us start breaking it down piece by piece. To better answer this question and provide a more thorough explanation, let us start by working through the following scenario:

**Example:** Imagine that you are tasked to assist in an internal on-site engagement for your company GreenHorn. You are immediately dropped into a Command Prompt session on a machine from within the internal network and have been tasked with enumerating the system. As per the rules of engagement, you have been stripped of any devices on your person and told that the firewall is blocking all outbound network traffic. You begin your enumeration on the system but need help remembering the syntax for a specific command you have in mind. You realize that you cannot reach the Internet by any means. Where can you find it?

Although this scenario might seem slightly exaggerated, there will be scenarios similar to this one as an attacker where our network access will be heavily limited, monitored, or strictly unavailable. Sometimes, we do not have every command and all parameters and syntax memorized; however, we will still be expected to perform

 even under these limitations. In instances where we are expected to perform, we will need alternate ways to gather the information we need instead of relying on the Internet as a quick fix to our problems. Now that we have our scenario, let us look back and break down our original question:

**Why does the help utility exist?**

The help utility serves as an offline manual for CMD and DOS compatible Windows operating system commands. Offline refers to the fact that this utility can be used on a system without network access. For those familiar with the Linux Fundamentals Module, this utility is very similar to the Man pages on Linux-based systems. Now that we understand why the help utility exists, we can cover the second part of the original question:

**What use does it serve today when access to the Internet is so prevalent?**

As shown in our scenario, there will be times when we may not have direct access to the Internet. The help utility is meant to bridge that gap when we need assistance with commands or specific syntax for said commands on our system and may not have the external resources available to ask for help. This does not imply that the Internet is not a valuable tool to use in engagements. However, if we do not have the luxury of searching for answers to our questions, we need some way to retrieve said information.

### Where Can You Find Additional Help?

In the previous section, we discussed the importance of utilizing the help system built into the Command Prompt, especially in an environment where external network traffic is non-existent or limited. However, assuming we have access to the Internet, there are dozens of online resources at our disposal for additional help regarding the Command Prompt. As stated before, the Internet is an extremely valuable tool and should be utilized to its fullest extent, especially if unrestricted access exists. To help enhance our understanding of CMD and alleviate some of the time sink involved with searching for material, here are a couple of CMD.exe command references where we can learn more about what can be done with our command shell.

- [Microsoft Documentation](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands) has a complete listing of the commands that can be issued within the command-line interpreter as well as detailed descriptions of how to use them. Think of it as an online version of the Man pages.
- [ss64](https://ss64.com/) Is a handy quick reference for anything command-line related, including cmd, PowerShell, Bash, and more.

This is a partial list of resources; however, these should provide a good baseline for working with the Command Prompt.

### Basic Tips & Tricks

Now that we have a general understanding of how we can obtain help from external resources, let's finish off strong by introducing some essential tips and tricks for interacting with the Command Prompt.

#### Clear Your Screen

There are times during our interaction with the command prompt when the amount of output provided to us through multiple commands overcrowding the screen and becomes an unusable mess of information. In this case, we need some way to clear the screen and provide us with an empty prompt. We can use the command `cls` to clear our terminal window of our previous results. This comes in handy when we need to refresh our screen and want to avoid fighting to read the terminal and figuring out where our current output starts and the old input ends.

#### History

Previously, we expanded upon clearing the output from the Command Prompt session using `cls`. Although that information has been cleared from the screen's output, we can still retrieve the commands that were run up to that point. This is due to a nifty feature built into the Command Prompt known as Command History.

Command history is a dynamic thing. It allows us to view previously ran commands in our Command Prompt's current active session. To do this, CMD provides us with several different methods of interacting with our command history. For example, we can use the arrow keys to move up and down through our history, the page up and page down keys, and if working on a physical Windows host, you can use the function keys to interact with your session history. The last way we can view our history is by utilizing the command `doskey /history`. Doskey is an MS-DOS utility that keeps a history of commands issued and allows them to be referenced again.

```

C:\htb> doskey /history

systeminfo
ipconfig /all
cls
ipconfig /all
systeminfo
cls
history
help
doskey /history
ping 8.8.8.8
doskey /history

```

From the output provided above, we can view a list of commands that were run before our original command. This is important and incredibly useful, especially if you are constantly clearing your screen and need to rerun a previous command to collect its output. Interacting and viewing all previously run commands will save you extra time, energy, and heartache.

#### Useful Keys & Commands for Terminal History

It would be helpful to have some way of remembering some of the key functionality provided by our terminal history. With this in mind, the table below shows a list of some of the most valuable functions and commands that can be run to interact with our session history. This list is not exhaustive. For

 example, the function keys F1 - F9 all serve a purpose when working with history.

| Key/Command       | Description                                                                                     |
|-------------------|-------------------------------------------------------------------------------------------------|
| doskey /history   | doskey /history will print the session's command history to the terminal or output it to a file when specified. |
| page up           | Places the first command in our session history to the prompt.                                 |
| page down         | Places the last command in history to the prompt.                                               |
| ⇧                 | Allows us to scroll up through our command history to view previously run commands.             |
| ⇩                 | Allows us to scroll down to our most recent commands run.                                       |
| ⇨                 | Types the previous command to prompt one character at a time.                                    |
| ⇦                 | N/A                                                                                             |
| F3                | Will retype the entire previous entry to our prompt.                                             |
| F5                | Pressing F5 multiple times will allow you to cycle through previous commands.                   |
| F7                | Opens an interactive list of previous commands.                                                 |
| F9                | Enters a command to our prompt based on the number specified. The number corresponds to the commands place in our history. |

One thing to remember is that unlike Bash or other shells, CMD does not keep a persistent record of the commands you issue through sessions. So once you close that instance, that history is gone. To save a copy of our issued commands, we can use `doskey` again to output the history to a file, show it on the screen, and then copy it.

#### Exit a Running Process

At some point in our journey working with the Command Prompt, there will be times when we will need to be able to interrupt an actively running process, effectively killing it. This can be due to many different factors. However, a lot of the time, we might have the information that we need from a currently running command or find ourselves dealing with an application that's locking up unexpectedly. Thus, we need some way of interrupting our current session and any process running in it. Take the following as an example:

```

C:\htb> ping 8.8.8.8

Pinging 8.8.8.8 with 32 bytes of data:
Reply from 8.8.8.8: bytes=32 time=22ms TTL=114
Reply from 8.8.8.8: bytes=32 time=25ms TTL=114

Ping statistics for 8.8.8.8:
Packets: Sent = 2, Received = 2, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 22ms, Maximum = 25ms, Average = 23ms
Control-C
^C

```

When running a command or process we want to interrupt, we can do so by pressing the `ctrl+c` key combination. As previously stated, this is useful for stopping a currently running process that may be non-responsive or just something we want to be completed immediately. Remember that whatever was running will be incomplete and may need more time to close itself out properly, so always be wary of what you are interrupting.

Now that we understand how to utilize the command prompt and its basic help functionality let us keep pressing forward and look at how we can begin navigating our system through the Command Prompt.
```
