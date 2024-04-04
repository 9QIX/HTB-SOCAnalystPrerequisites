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

````

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
```

```
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
```
````
