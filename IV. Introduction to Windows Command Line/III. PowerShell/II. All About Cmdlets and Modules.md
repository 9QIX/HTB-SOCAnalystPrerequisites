# All About Cmdlets and Modules

In this section, we will cover the following:

- What are cmdlets and Modules?
- How do we interact with them?
- How do we install and load new modules from the web?

Understanding these questions is crucial when utilizing PowerShell as both a sysadmin and pentester. PowerShells' ability to be modular and expandable makes it a powerhouse tool to have in our kit. Let us dive into what cmdlets and modules are.

## Cmdlets

A cmdlet as defined by Microsoft is:

"a single-feature command that manipulates objects in PowerShell."

Cmdlets follow a Verb-Noun structure which often makes it easier for us to understand what any given cmdlet does. With `Test-WSMan`, we can see the verb is `Test` and the Noun is `Wsman`. The verb and noun are separated by a dash (`-`). After the verb and noun, we would use the options available to us with a given cmdlet to perform the desired action. Cmdlets are similar to functions used in PowerShell code or other programming languages but have one significant difference. Cmdlets are not written in PowerShell. They are written in C# or another language and then compiled for use. As we saw in the last section, we can use the `Get-Command` cmdlet to view the available applications, cmdlets, and functions, along with a trait labeled "CommandType" that can help us identify its type.

If we want to see the options and functionality available to us with a specific cmdlet, we can use the `Get-Help` cmdlet as well as the `Get-Member` cmdlet.

## PowerShell Modules

A PowerShell module is structured PowerShell code that is made easy to use & share. As mentioned in the official Microsoft docs, a module can be made up of the following:

- Cmdlets
- Script files
- Functions
- Assemblies
- Related resources (manifests and help files)

Through this section, we are going to use the PowerView project to examine what makes up a module and how to interact with them. `PowerView.ps1` is part of a collection of PowerShell modules organized in a project called PowerSploit created by the PowerShellMafia to provide penetration testers with many valuable tools to use when testing Windows Domain/Active Directory environments. Though we may notice this project has been archived, many of the included tools are still relevant and useful in pen-testing today (written in August 2022). We will not extensively cover the usage and implementation of PowerSploit in this module. We will just be using it as a reference to understand PowerShell better. The use of PowerSploit to Enumerate & Attack Windows Domain environments is covered in great depth in the module Active Directory Enumeration & Attacks.

### PowerSploit

#### PowerSploit.psd1

A PowerShell data file (`.psd1`) is a Module manifest file. Contained in a manifest file we can often find:

- Reference to the module that will be processed
- Version numbers to keep track of major changes
- The GUID
- The Author of the module
- Copyright
- PowerShell compatibility information
- Modules & cmdlets included
- Metadata

#### PowerSploit.psm1

A PowerShell script module file (`.psm1`) is simply a script containing PowerShell code. Think of this as the meat of a module.

Contents of `PowerSploit.psm1`:

```powershell
Get-ChildItem $PSScriptRoot | ? { $_.PSIsContainer -and !('Tests','docs' -contains $_.Name) } | % { Import-Module $_.FullName -DisableNameChecking }
```

The `Get-ChildItem` cmdlet gets the items in the current directory (represented by the `$PSScriptRoot` automatic variable), and the `Where-Object` cmdlet (aliased as the `?` character) filters those down to only the items that are folders and do not have the names "Tests" or "docs". Finally, the `ForEach-Object` cmdlet (aliased as the `%` character) executes the `Import-Module` cmdlet against each of those remaining items, passing the `DisableNameChecking` parameter to prevent errors if the module contains cmdlets or functions with the same names as cmdlets or functions in the current session.

## Using PowerShell Modules

Once we decide what PowerShell module we want to use, we will have to determine how and from where we will run it. We also must consider if the chosen module and scripts are already on the host or if we need to get them on to the host. `Get-Module` can help us determine what modules are already loaded.

```powershell
Get-Module
```

```
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     0.0        chocolateyProfile                   {TabExpansion, Update-SessionEnvironment, refreshenv}
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Script     0.7.3.1    posh-git                            {Add-PoshGitToProfile, Add-SshKey, Enable-GitColors, Expan...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

```powershell
Get-Module -ListAvailable
```

```
Directory: C:\Users\tru7h\Documents\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     1.1.0      PSSQLite                            {Invoke-SqliteBulkCopy, Invoke-SqliteQuery, New-SqliteConn...


    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     1.0.1      Microsoft.PowerShell.Operation.V... {Get-OperationValidation, Invoke-OperationValidation}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Get-Package, Get-PackageProvider, Get-Packa...
Script     3.4.0      Pester                              {Describe, Context, It, Should...}
Script     1.0.0.1    PowerShellGet                       {Install-Module, Find-Module, Save-Module, Update-Module...}
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Set-PSReadLineKeyHandler, Remov...
```

The `-ListAvailable` modifier will show us all modules we have installed but not loaded into our session.

We have already transferred the desired module or scripts onto a target Windows host. We will then need to run them. We can start through the use of the `Import-Module` cmdlet.

### Using Import-Module

The `Import-Module` cmdlet allows us to add a module to the current PowerShell session.

```powershell
Get-Help Import-Module

NAME
    Import-Module

SYNOPSIS
    Adds modules to the current session.


SYNTAX
    Import-Module [-Assembly] <System.Reflection.Assembly[]> [-Alias <System.String[]>] [-ArgumentList
    <System.Object[]>] [-AsCustomObject] [-Cmdlet <System.String[]>] [-DisableNameChecking] [-Force] [-Function
    <System.String[]>] [-Global] [-NoClobber] [-PassThru] [-Prefix <System.String>] [-Scope {Local | Global}]
    [-Variable <System.String[]>] [<CommonParameters>]

    Import-Module [-Name] <System.String[]> [-Alias <System.String[]>] [-ArgumentList <System.Object[]>]
    [-AsCustomObject] [-CimNamespace <System.String>] [-CimResourceUri <System.Uri>] -CimSession
    <Microsoft.Management.Infrastructure.CimSession> [-Cmdlet <System.String[]>] [-DisableNameChecking] [-Force]
    [-Function <System.String[]>] [-Global] [-MaximumVersion <System.String>] [-MinimumVersion <System.Version>]
    [-NoClobber] [-PassThru] [-Prefix <System.String>] [-RequiredVersion <System.Version>] [-Scope {Local |
Global}]
    [-Variable <System.String[]>] [<CommonParameters>]

<SNIP>
```

To understand the idea of importing the module into our current PowerShell session, we can attempt to run a cmdlet (`Get-NetLocalgroup`) that is part of PowerSploit. We will get an error message when attempting to do this without importing a module. Once we successfully import the PowerSploit module (it has been placed on the target host's Desktop for our use), many cmdlets will be available to us, including `Get-NetLocalgroup`. See this in action in the clip below:

```powershell
Import-Module .\PowerSploit.psd1
Get-NetLocalgroup
```

```
ComputerName GroupName                           Comment
------------ ---------                           -------
WS01         Access Control Assistance Operators Members of this group can remotely query authorization attributes a...
WS01         Administrators                      Administrators have complete and unrestricted access to the compute...
WS01         Backup Operators                    Backup Operators can override security restrictions for the sole pu...
WS01         Cryptographic Operators             Members are authorized to perform cryptographic operations.
WS01         Distributed COM Users               Members are allowed to launch, activate and use Distributed COM obj...
WS01         Event Log Readers                   Members of this group can read event logs from local machine
WS01         Guests                              Guests have the same access as members of the Users group by defaul...
WS01         Hyper-V Administrators              Members of this group have complete and unrestricted access to all ...
WS01         IIS_IUSRS                           Built-in group used by Internet Information Services.
WS01         Network Configuration Operators     Members in this group can have some administrative privileges to ma...
WS01         Performance Log Users               Members of this group may schedule logging of performance counters,...
WS01         Performance Monitor Users           Members of this group can access performance counter data locally a...
WS01         Power Users                         Power Users are included for backwards compatibility and possess li...
WS01         Remote Desktop Users                Members in this group are granted the right to logon remotely
WS01         Remote Management Users             Members of this group can access WMI resources over management prot...
WS01         Replicator                          Supports file replication in a domain
WS01         System Managed Accounts Group       Members of this group are managed by the system.
WS01         Users                               Users are prevented from making accidental or intentional system-wi...
```

Notice how at the beginning of the clip, `Get-NetLocalgroup` was not recognized. This event happened because it is not included in the default module path. We see where the default module path is by listing the environment variable `PSModulePath`.

```powershell
$env:PSModulePath
```

```
C:\Users\htb-student\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modules
```

When the `PowerSploit.psd1` module is imported, the `Get-NetLocalgroup` function is recognized. This happens because several modules are included when we load `PowerSploit.psd1`. It is possible to permanently add a module or several modules by adding the files to the referenced directories in the `PSModulePath`. This action makes sense if we were using a Windows OS as our primary attack host, but on an engagement, our time would be better off just transferring specific scripts over to the attack host and importing them as needed.

### Execution Policy

An essential factor to consider when attempting to use PowerShell scripts and modules is PowerShell's execution policy. As outlined in Microsoft's official documentation, an execution policy is not a security control. It is designed to give IT admins a tool to set parameters and safeguards for themselves.

```powershell
Import-Module .\PowerSploit.psd1
```

```
Import-Module : File C:\Users\Users\htb-student\PowerSploit.psm1
cannot be loaded because running scripts is disabled on this system. For more information, see
about_Execution_Policies at https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ Import-Module .\PowerSploit.psd1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [Import-Module], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.PowerShell.Commands.ImportModuleCommand
```

The host's execution policy makes it so that we cannot run our script. We can get around this, however. First, let us check our execution policy settings.

```powershell
Get-ExecutionPolicy
```

```
Restricted
```

Our current setting restricts what the user can do. If we want to change the setting, we can do so with the `Set-ExecutionPolicy` cmdlet.

```powershell
Set-ExecutionPolicy undefined
```

By setting the policy to undefined, we are telling PowerShell that we do not wish to limit our interactions. Now we should be able to import and run our script.

```powershell
Import-Module .\PowerSploit.psd1

Get-Module
```

```
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Check...
Manifest   3.0.0.0    Microsoft.PowerShell.Security       {ConvertFrom-SecureString, Conver...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Vari...
Script     3.0.0.0    PowerSploit                         {Add-Persistence, Add-ServiceDacl...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PS...
```

Looking at our loaded modules, we can see that we successfully loaded PowerSploit. Now we can use the tools as needed.

Note: As a sysadmin, these kinds of changes are common and should always be reverted once we are done with work. As a pentester, us making a change like this and not reverting it could indicate to a defender that the host has been compromised. Be sure to check that we clean up after our actions. Another way we can bypass the execution policy and not leave a persistent change is to change it at the process level using `-scope`.

```powershell
Set-ExecutionPolicy -scope Process
Get-ExecutionPolicy -list
```

```
Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process          Bypass
  CurrentUser       Undefined
 LocalMachine          Bypass
```

By changing it at the Process level, our change will revert once we close the PowerShell session. Keep the execution policy in mind when working with scripts and new modules. Of course, we want to look at the scripts we are trying to load first to ensure they are safe for use. As penetration testers, we may run into times when we need to be creative about how we bypass the Execution Policy on a host. This [blog post](https://blog.netspi.com/15-ways-to-bypass-the-powershell-execution-policy/) has some creative ways that we have used on real-world engagements with great success.

## Calling Cmdlets and Functions From Within a Module

If we wish to see what aliases, cmdlets, and functions an imported module brought to the session, we can use `Get-Command -Module <modulename>` to enlighten us.

```powershell
Get-Command -Module PowerSploit
```

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Invoke-ProcessHunter                               3.0.0.0    PowerSploit
Alias           Invoke-ShareFinder                                 3.0.0.0    PowerSploit
Alias           Invoke-ThreadedFunction                            3.0.0.0    PowerSploit
Alias           Invoke-UserHunter                                  3.0.0.0    PowerSploit
Alias           Request-SPNTicket                                  3.0.0.0    PowerSploit
Alias           Set-ADObject                                       3.0.0.0    PowerSploit
Function        Add-Persistence                                    3.0.0.0    PowerSploit
Function        Add
```
