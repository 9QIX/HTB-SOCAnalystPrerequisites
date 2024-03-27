# Windows Services & Processes

## Windows Services

Services are a major component of the Windows operating system. They allow for the creation and management of long-running processes. Windows services can be started automatically at system boot without user intervention. These services can continue to run in the background even after the user logs out of their account on the system.

Applications can also be created to install as a service, such as a network monitoring application installed on a server. Services on Windows are responsible for many functions within the Windows operating system, such as networking functions, performing system diagnostics, managing user credentials, controlling Windows updates, and more.

Windows services are managed via the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in.

This add-in provides a GUI interface for interacting with and managing services and displays information about each installed service. This information includes the service Name, Description, Status, Startup Type, and the user that the service runs under.

It is also possible to query and manage services via the command line using `sc.exe` using PowerShell cmdlets such as `Get-Service`.

```powershell
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl
```

```plaintext
Name                : AdobeARMservice
DisplayName         : Adobe Acrobat Update Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : Appinfo
DisplayName         : Application Information
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {RpcSs, ProfSvc}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess, Win32ShareProcess
```

Let's break down the command:

- `Get-Service`: This cmdlet retrieves information about all services installed on the system.

- `|`: The pipe symbol takes the output of the `Get-Service` command and passes it as input to the next command.

- `? {$_.Status -eq "Running"}`: This is a filter using the `Where-Object` cmdlet (`?` is an alias for `Where-Object`). It selects only those services where the `Status` property is equal to "Running".

- `|`: Another pipe symbol to pass the filtered output to the next command.

- `select -First 2`: This cmdlet selects the first two items from the filtered list of running services.

- `|fl`: Another pipe symbol followed by `fl`, which is an alias for the `Format-List` cmdlet. It formats the selected services' properties and values in a detailed list format.

Service statuses can appear as Running, Stopped, or Paused, and they can be set to start manually, automatically, or on a delay at system boot. Services can also be shown in the state of Starting or Stopping if some action has triggered them to either start or stop. Windows has three categories of services: Local Services, Network Services, and System Services. Services can usually only be created, modified, and deleted by users with administrative privileges. Misconfigurations around service permissions are a common privilege escalation vector on Windows systems.

In Windows, we have some critical system services that cannot be stopped and restarted without a system restart. If we update any file or resource in use by one of these services, we must restart the system.

| Service                     | Description                                                                                                                                                                                                                                             |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `smss.exe`                  | Session Manager SubSystem. Responsible for handling sessions on the system.                                                                                                                                                                             |
| `csrss.exe`                 | Client Server Runtime Process. The user-mode portion of the Windows subsystem.                                                                                                                                                                          |
| `wininit.exe`               | Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.                                                                                                        |
| `logonui.exe`               | Used for facilitating user login into a PC                                                                                                                                                                                                              |
| `lsass.exe`                 | The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.                                                                |
| `services.exe`              | Manages the operation of starting and stopping services.                                                                                                                                                                                                |
| `winlogon.exe`              | Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.                                                                                                        |
| `System`                    | A background system process that runs the Windows kernel.                                                                                                                                                                                               |
| `svchost.exe` with RPCSS    | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).                                |
| `svchost.exe` with Dcom/PnP | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services. |

[This link](https://docs.microsoft.com/en-us/windows/win32/services/services) has a list of Windows components, including key services.

## Processes

Processes run in the background on Windows systems. They either run automatically as part of the Windows operating system or are started by other installed applications.

Processes associated with installed applications can often be terminated without causing a severe impact on the operating system. Certain processes are critical and, if terminated, will stop certain components of the operating system from running properly. Some examples include the Windows Logon Application, System, System Idle Process, Windows Start-Up Application, Client Server Runtime, Windows Session Manager, Service Host, and Local Security Authority Subsystem Service (LSASS) process.

### Local Security Authority Subsystem Service (LSASS)

`lsass.exe` is the process that is responsible for enforcing the security policy on Windows systems. When a user attempts to log on to the system, this process verifies their log on attempt and creates access tokens based on the user's permission levels. LSASS is also responsible for user account password changes. All events associated with this process (logon/logoff attempts, etc.) are logged within the Windows Security Log. LSASS is an extremely high-value target as several tools exist to extract both cleartext and hashed credentials stored in memory by this process.

## Sysinternals Tools

The SysInternals Tools suite is a set of portable Windows applications that can be used to administer Windows systems (for the most part without requiring installation). The tools can be either downloaded from the Microsoft website or by loading them directly from an internet-accessible file share by typing `\\live.sysinternals.com\tools` into a Windows Explorer window.

For example, we can run `procdump.exe` directly from this share without downloading it directly to disk.

```powershell
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula
```

```plaintext
ProcDump v9.0 - Sysinternals process dump utility
Copyright (C) 2009-2017 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

Monitors a process and writes a dump file when the process exceeds the
specified criteria or has an exception.

Capture Usage:
   procdump.exe [-mm] [-ma] [-mp] [-mc Mask] [-md Callback_DLL] [-mk]
                [-n Count]
                [-s Seconds]
                [-c|-cl CPU_Usage [-u]]
                [-m|-ml Commit_Usage]
                [-p|-pl Counter_Threshold]
                [-h]
                [-e [1 [-g] [-b]]]
                [-l]
                [-t]
                [-f  Include_Filter, ...]
                [-fx Exclude_Filter, ...]
                [-o]
                [-r [1..5] [-a]]
                [-wer]
                [-64]
                {
                 {{[-w] Process_Name | Service_Name | PID} [Dump_File | Dump_Folder]}
                |
                 {-x Dump_Folder Image_File [Argument, ...]}
                }
```

This command appears to be attempting to execute the `procdump.exe` tool located at `\\live.sysinternals.com\tools\procdump.exe` on a remote system.

Here's what each part of the command means:

- `\\live.sysinternals.com\tools\procdump.exe`: This is the path to the `procdump.exe` tool located on a network share named `live.sysinternals.com`. Sysinternals is a collection of advanced system utilities and technical information developed by Microsoft, and `procdump.exe` is one of the utilities provided by Sysinternals for creating process dump files.

- `-accepteula`: This option is used to automatically accept the End-User License Agreement (EULA) for `procdump.exe`. By including this option, the command will not prompt the user to accept the EULA interactively.

Overall, this command seems to be trying to run `procdump.exe` from a network share, and it's accepting the EULA automatically. It's likely being used for diagnostic or troubleshooting purposes.

The suite includes tools such as Process Explorer, an enhanced version of Task Manager, and Process Monitor, which can be used to monitor file system, registry, and network activity related to any process running on the system. Some additional tools are TCPView, which is used to monitor internet activity, and PSExec, which can be used to manage/connect to systems via the SMB protocol remotely.

These tools can be useful for penetration testers to, for example, discover interesting processes and possible privilege escalation paths as well as for lateral movement.

## Task Manager

Windows Task Manager is a powerful tool for managing Windows systems. It provides information about running processes, system performance, running services, startup programs, logged-in users/logged in user processes, and services. Task Manager can be opened by right-clicking on the taskbar and selecting Task Manager, pressing ctrl + shift + Esc, pressing ctrl + alt + del and selecting Task Manager, opening the start menu and typing Task Manager, or typing taskmgr from a CMD or PowerShell console.

![alt text](/Images/image-22.png)

| Tab             | Description                                                                                                                                                                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Processes tab   | Shows a list of running applications and background processes along with the CPU, memory, disk, network, and power usage for each.                                                                                                                            |
| Performance tab | Shows graphs and data such as CPU utilization, system uptime, memory usage, disk and networking, and GPU usage. We can also open the Resource Monitor, which gives us a much more in-depth view of the current CPU, Memory, Disk, and Network resource usage. |

![alt text](/Images/image-23.png)

| Tab             | Description                                                                                                                        |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| App history tab | Shows resource usage for the current user account for each application for a period of time.                                       |
| Startup tab     | Shows which applications are configured to start at boot as well as the impact on the startup process.                             |
| Users tab       | Shows logged in users and the processes/resource usage associated with their session.                                              |
| Details tab     | Shows the name, process ID (PID), status, associated username, CPU, and memory usage for each running application.                 |
| Services tab    | Shows the name, PID, description, and status of each installed service. The Services add-in can be accessed from this tab as well. |

## Process Explorer

Process Explorer is a part of the Sysinternals tool suite. This tool can show which handles and DLL processes are loaded when a program runs. Process Explorer shows a list of currently running processes, and from there, we can see what handles the process has selected in one view or the DLLs and memory-swapped files that have been loaded in another view. We can also search within the tool to show which processes tie back to a specific handle or DLL. The tool can also be used to analyze parent-child process relationships to see what child processes are spawned by an application and help troubleshoot any issues such as orphaned processed that can be left behind when a process is terminated.
