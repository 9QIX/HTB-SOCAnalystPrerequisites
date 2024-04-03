````markdown
# CMD Vs. PowerShell

Up to this point, we have discussed the built-in Windows Command-line interpreter cmd.exe. Moving forward, we will look at Window's modern successor to CMD, PowerShell. This section will cover what PowerShell is, the differences between PowerShell and CMD, how to get help within the CLI, and basic navigation within the CLI.

## Differences

PowerShell and CMD are included natively on any Windows host, so we may ask ourselves, "Why would I use one over the other?" Let's address this quickly. Below is a table with some differences between PowerShell and CMD.

### PowerShell and CMD Compared

| Feature             | CMD                                                                 | PowerShell                                                       |
| ------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Language            | Batch and basic CMD commands only.                                  | PowerShell can interpret Batch, CMD, PS cmdlets, and aliases.    |
| Command utilization | The output from one command cannot be passed into another directly. | The output from one command can be passed into another directly. |
| Command Output      | Text only                                                           | PowerShell outputs in object formatting.                         |
| Parallel Execution  | CMD must finish one command before running another.                 | PowerShell can multi-thread commands to run in parallel.         |

Most notably, PowerShell has been built to be extensible and to integrate with many other tools and functionality as needed. Most think of it as just another CLI, but it is much more. Did you know it is also a scripting language? While CMD has been the default command-line interpreter for Windows hosts only, PowerShell has been released as an open-source project and has an extensive offering of capabilities that support its use with Linux-based systems as well. Using the .NET framework has also made PowerShell capable of utilizing an object base model of interaction and output instead of text-based only.

## Why Choose PowerShell Over cmd.exe?

Why does PowerShell matter for IT admins, Offensive & Defensive Infosec pros?

PowerShell has become increasingly prominent among IT and Infosec professionals. It has widespread utility for System Administrators, Penetration Testers, SOC Analysts, and many other technical disciplines where ever Windows systems are administered. Consider IT admins and Windows system administrators administering IT environments made up of Windows servers, desktops (Windows 10 & 11), Azure, and Microsoft 365 cloud-based applications. Many of them are using PowerShell to automate tasks they must accomplish daily. Among some of these tasks are:

- Provisioning servers and installing server roles
- Creating Active Directory user accounts for new employees
- Managing Active Directory group permissions
- Disabling and deleting Active Directory user accounts
- Managing file share permissions
- Interacting with Azure AD and Azure VMs
- Creating, deleting, and monitoring directories & files
- Gathering information about workstations and servers
- Setting up Microsoft Exchange email inboxes for users (in the cloud &/or on-premises)

There are countless ways to use PowerShell from an IT admin context, and being mindful of that context can be helpful for us as penetration testers and even as defenders. As a sysadmin, PowerShell can provide us with much more capability than CMD. It is expandable, built for automation and scripting, has a much more robust security implementation, and can handle many different tasks that CMD simply cannot. As a pentester, many well-known capabilities are built into PowerShell. PowerShell's module import capability makes it easy to bring our tools into the environment and ensure they will work. However, from a stealth perspective, PowerShell's logging and history capability is powerful and will log more of our interactions with the host. So if we do not need PowerShell's capabilities and wish to be more stealthy, we should utilize CMD.

## Calling PowerShell

We can access PowerShell directly on a host through the peripherals attached to the local machine or through RDP over the network through various methods.

### Using Windows Search

We can type PowerShell in Windows Search to find and launch the PowerShell application and command console.

![Searching for PowerShell](search-powershell.png)

### Using the Windows Terminal Application

Windows Terminal is a newer terminal emulator application developed by Microsoft to give anyone using the Windows command line a way to access multiple different command line interfaces, systems, and sub-systems through a single app. This application will likely become the default terminal emulator on Windows operating systems.

![PowerShell in Windows Terminal](powershell-windows-terminal.png)

### Using Windows PowerShell ISE

The Windows PowerShell Integrated Scripting Environment (ISE) is like an IDE for PowerShell. It can make it easier to develop, debug and test the PowerShell scripts we create. Using PowerShell ISE can be incredibly useful when learning PowerShell.

![PowerShell ISE](powershell-ise.png)

### Using PowerShell in CMD

We can also launch PowerShell from within CMD. This action may seem trivial, but there will undoubtedly come a time when we can get a shell on a vulnerable Windows target's CLI via CMD and will benefit from attempting to use PowerShell to further our access on the host and across the network.

```powershell
PS C:\> powershell
```
````

## Taking a Look at the Shell

One of the first things we may examine when accessing PowerShell on a local or remote system is the prompt.

### PowerShell Prompt

```
PS C:\Users\htb-student> ipconfig

Ethernet adapter VMware Network Adapter VMnet8:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::adb8:3c9:a8af:114%25
   IPv4 Address. . . . . . . . . . . : 172.16.110.1
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . :
```

- `PS` is short for PowerShell, followed by the current working directory `C:\Users\htb-student>`.
- This is followed by the cmdlet or string we want to execute, `ipconfig`.
- Finally, below that, we see the output results of our command.

Also similar to CMD, PowerShell gives us many commands and cmdlets to utilize. Almost all commands that work in CMD will work in PowerShell. We will only cover some possible commands in this module. It is essential to understand that there is very little utility in memorizing commands. Focus more on understanding context, concepts, and what is possible. Memorization will naturally happen with time spent practicing and repetition.

## Get-Help

Using the Help function. If we want to see the options and functionality available to us with a specific cmdlet, we can use the `Get-Help` cmdlet.

### Using Get-Help

```powershell
PS C:\Users\htb-student> Get-Help Test-Wsman

NAME
    Test-WSMan

SYNTAX
    Test-WSMan [[-ComputerName] <string>] [-Authentication {None | Default | Digest | Negotiate | Basic | Kerberos |
    ClientCertificate | Credssp}] [-Port <int>] [-UseSSL] [-ApplicationName <string>] [-Credential <pscredential>]
    [-CertificateThumbprint <string>]  [<CommonParameters>]


ALIASES
    None


REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help Test-WSMan -Online" or
           go to https://go.microsoft.com/fwlink/?LinkId=141464.
```

`Get-Help` can give helpful information about a cmdlet. Notice that the `Syntax` output shows us several available options and additional keywords that can be used with each option. Aliases are also mentioned, essentially shorter names for our commands. We will discuss Aliases in more depth later in this section. The `Remarks` output provides us with further information about the cmdlet and even additional options we can use to learn more about the cmdlet. One of these additional options is `-online`, which will open a Microsoft docs webpage for the corresponding cmdlet if the host has Internet access.

![Get-Help Online](get-help-online.png)

We can also use a helpful cmdlet called `Update-Help` to ensure we have the most up-to-date information for each cmdlet on the Windows system.

### Using Update-Help

```powershell
PS C:\Windows\system32> Update-Help
```

Notice how much more information was populated regarding `Test-Wsman` after running `Update-Help`. Feel free to compare this output to the output shown earlier when we first covered `Get-Help`.
