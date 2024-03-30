```markdown
## Introduction

The built-in command shell CMD.exe and PowerShell are two implementations included in all Windows hosts. These tools provide direct access to the operating system, automate routine tasks, and provide the user with granular control of any aspect of the computer and installed applications. This module will give us the knowledge, skills, and abilities to effectively administer Windows hosts via the command line.

From a penetration testing perspective, we will learn how to utilize built-in Windows tools and commands and third-party scripts and applications to help with reconnaissance, exploitation, and exfiltration of data from within a Windows environment as we move into more advanced modules within HTB Academy.

## Command Prompt Vs. PowerShell

There are some key differences between Windows Command Prompt and PowerShell, which we will see throughout this module. One key difference is that you can run Command Prompt commands from a PowerShell console, but to run PowerShell commands from a Command Prompt, you would have to preface the command with `powershell` (i.e., `powershell get-alias`). The following table outlines some other key differences.

| PowerShell                                                                 | Command Prompt                                        |
| -------------------------------------------------------------------------- | ----------------------------------------------------- |
| Introduced in 2006                                                         | Introduced in 1981                                    |
| Can run both batch commands and PowerShell cmdlets                         | Can only run batch commands                           |
| Supports the use of command aliases                                        | Does not support command aliases                      |
| Cmdlet output can be passed to other cmdlets                               | Command output cannot be passed to other commands     |
| All output is in the form of an object                                     | Output of commands is text                            |
| Able to execute a sequence of cmdlets in a script                          | A command must finish before the next command can run |
| Has an Integrated Scripting Environment (ISE)                              | Does not have an ISE                                  |
| Can access programming libraries because it is built on the .NET framework | Cannot access these libraries                         |
| Can be run on Linux systems                                                | Can only be run on Windows systems                    |

As we can see, the Command Prompt is a much more static way of interacting with the operating system, while PowerShell is a powerful scripting language that can be used for a wide variety of tasks and to create simple and very complex scripts.

## Scenario

We will use a scenario through this module to help keep the topics in scope and provide insight into how these tools and commands can aid our mission.

Consider this scenario:

We are a system administrator looking to broaden our horizons and dip our toes into pentesting. Before we approach our manager and Internal Red Team Lead to see about apprenticing, we must first practice and gain a fundamental understanding of Windows primary command line interfaces: PowerShell and Command Prompt. Soon they will have no choice but to accept us as a certified Command Line Ninja and grant us a seat at the table.

## Connection Instructions

For this module, you will have access to several Windows hosts from which you can perform any actions needed to complete the lab exercises. Since we are working in a pure CLI-based module, this challenge will use SSH only to connect with the targets.

To connect to the target hosts as the user via SSH, utilize the following format:
```

ssh htb-student@<IP-Address>

```

```
