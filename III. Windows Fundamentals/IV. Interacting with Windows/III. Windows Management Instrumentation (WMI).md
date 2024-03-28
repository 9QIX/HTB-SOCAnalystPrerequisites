# Windows Management Instrumentation (WMI)

## WMI Overview

WMI is a subsystem of PowerShell that provides system administrators with powerful tools for system monitoring. The goal of WMI is to consolidate device and application management across corporate networks. WMI is a core part of the Windows operating system and has come pre-installed since Windows 2000. It is made up of the following components:

| Component Name     | Description                                                                                                                                                                        |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| WMI service        | The Windows Management Instrumentation process, which runs automatically at boot and acts as an intermediary between WMI providers, the WMI repository, and managing applications. |
| Managed objects    | Any logical or physical components that can be managed by WMI.                                                                                                                     |
| WMI providers      | Objects that monitor events/data related to a specific object.                                                                                                                     |
| Classes            | These are used by the WMI providers to pass data to the WMI service.                                                                                                               |
| Methods            | These are attached to classes and allow actions to be performed. For example, methods can be used to start/stop processes on remote machines.                                      |
| WMI repository     | A database that stores all static data related to WMI.                                                                                                                             |
| CIM Object Manager | The system that requests data from WMI providers and returns it to the application requesting it.                                                                                  |
| WMI API            | Enables applications to access the WMI infrastructure.                                                                                                                             |
| WMI Consumer       | Sends queries to objects via the CIM Object Manager.                                                                                                                               |

## Uses of WMI

Some of the uses for WMI are:

- Status information for local/remote systems
- Configuring security settings on remote machines/applications
- Setting and changing user and group permissions
- Setting/modifying system properties
- Code execution
- Scheduling processes
- Setting up logging

These tasks can all be performed using a combination of PowerShell and the WMI Command-Line Interface (WMIC).

## WMIC Command-Line Interface

WMI can be run via the Windows command prompt by typing `WMIC` to open an interactive shell or by running a command directly such as `wmic computersystem get name` to get the hostname. We can view a listing of WMIC commands and aliases by typing `WMIC /?`.

```powershell
C:\htb> wmic /?
# WMIC command help output

WMIC is deprecated.

[global switches] <command>

The following global switches are available:
/NAMESPACE           Path for the namespace the alias operate against.
/ROLE                Path for the role containing the alias definitions.
/NODE                Servers the alias will operate against.
/IMPLEVEL            Client impersonation level.
/AUTHLEVEL           Client authentication level.
/LOCALE              Language id the client should use.
/PRIVILEGES          Enable or disable all privileges.
/TRACE               Outputs debugging information to stderr.
/RECORD              Logs all input commands and output.
/INTERACTIVE         Sets or resets the interactive mode.
/FAILFAST            Sets or resets the FailFast mode.
/USER                User to be used during the session.
/PASSWORD            Password to be used for session login.
/OUTPUT              Specifies the mode for output redirection.
/APPEND              Specifies the mode for output redirection.
/AGGREGATE           Sets or resets aggregate mode.
/AUTHORITY           Specifies the <authority type> for the connection.
/?[:<BRIEF|FULL>]    Usage information.
```

An example command lists information about the operating system:

```powershell
C:\htb> wmic os list brief

BuildNumber  Organization  RegisteredUser  SerialNumber             SystemDirectory      Version
19041                      Owner           00123-00123-00123-AAOEM  C:\Windows\system32  10.0.19041
```

WMIC uses aliases and associated verbs, adverbs, and switches. It provides various options for data retrieval and manipulation.

## Using WMI with PowerShell

WMI can be used with PowerShell by using the `Get-WmiObject` module. This module is used to get instances of WMI classes or information about available classes. This module can be used against local or remote machines.

Here's an example of getting information about the operating system:

```plaintext
PS C:\htb> Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumber,Version | ft
# Output of OS information using PowerShell
```

Additionally, the `Invoke-WmiMethod` module is used to call the methods of WMI objects. For example, renaming a file:

```plaintext
PS C:\htb> Invoke-WmiMethod -Path "CIM_DataFile.Name='C:\users\public\spns.csv'" -Name Rename -ArgumentList "C:\Users\Public\kerberoasted_users.csv"
# Output of the method invocation
```

## Conclusion

This section provides a brief overview of WMI, WMIC, and combining WMIC and PowerShell. WMI has a wide variety of uses for both blue team and red team operators. Later sections of this course will show some ways that WMI can be leveraged offensively for both enumeration and lateral movement.
