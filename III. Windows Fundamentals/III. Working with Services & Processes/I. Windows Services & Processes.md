## Windows Services

Services are a fundamental component of the Windows operating system, allowing the creation and management of long-running processes. These services can start automatically at system boot and continue running in the background even after user logout.

Applications can also be installed as services, such as network monitoring tools on servers. Windows services handle various functions within the operating system, including networking, system diagnostics, user credential management, Windows updates, and more.

### Managing Services

Windows services are managed through the Service Control Manager (SCM) system, accessible via the `services.msc` MMC add-in. This provides a graphical interface to interact with and manage services, displaying information such as service name, description, status, startup type, and user context.

Services can also be managed via command-line tools like `sc.exe` and PowerShell cmdlets such as `Get-Service`.

```powershell
# Example PowerShell command to list running services
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl

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

Service statuses can be Running, Stopped, or Paused, with startup types set as manual, automatic, or delayed at system boot. Services can also be in the process of Starting or Stopping. Windows categorizes services into Local, Network, and System Services, usually manageable only by users with administrative privileges.

| Service                   | Description                                                                                                                                                                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| smss.exe                  | Session Manager SubSystem. Responsible for handling sessions on the system.                                                                                                                                                                             |
| csrss.exe                 | Client Server Runtime Process. The user-mode portion of the Windows subsystem.                                                                                                                                                                          |
| wininit.exe               | Starts the Wininit file .ini file that lists all of the changes to be made to Windows when the computer is restarted after installing a program.                                                                                                        |
| logonui.exe               | Used for facilitating user login into a PC.                                                                                                                                                                                                             |
| lsass.exe                 | The Local Security Authentication Server verifies the validity of user logons to a PC or server. It generates the process responsible for authenticating users for the Winlogon service.                                                                |
| services.exe              | Manages the operation of starting and stopping services.                                                                                                                                                                                                |
| winlogon.exe              | Responsible for handling the secure attention sequence, loading a user profile on logon, and locking the computer when a screensaver is running.                                                                                                        |
| System                    | A background system process that runs the Windows kernel.                                                                                                                                                                                               |
| svchost.exe with RPCSS    | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Remote Procedure Call (RPC) Service (RPCSS).                                |
| svchost.exe with Dcom/PnP | Manages system services that run from dynamic-link libraries (files with the extension .dll) such as "Automatic Updates," "Windows Firewall," and "Plug and Play." Uses the Distributed Component Object Model (DCOM) and Plug and Play (PnP) services. |

Misconfigurations in service permissions are common vectors for privilege escalation on Windows systems.

### Critical System Services

Some critical system services cannot be stopped and restarted without system restart. Examples include:

- **smss.exe**: Session Manager SubSystem
- **csrss.exe**: Client Server Runtime Process
- **wininit.exe**: Windows Start-Up Application
- **logonui.exe**: User login facilitator
- **lsass.exe**: Local Security Authentication Server
- **services.exe**: Manages service operations
- **winlogon.exe**: Handles secure attention sequence and user profile loading
- **System**: Background system process running the Windows kernel
- **svchost.exe with RPCSS**: Manages system services using Remote Procedure Call (RPC) Service
- **svchost.exe with Dcom/PnP**: Manages system services using Distributed Component Object Model (DCOM) and Plug and Play (PnP) services

### Processes

Processes run in the background on Windows systems, either automatically as part of the OS or started by installed applications. Critical processes like Windows Logon Application and LSASS are essential for system functionality.

**LSASS (Local Security Authority Subsystem Service)**: Enforces security policy, verifies user logon attempts, and manages user account password changes. High-value target due to credential storage in memory.

### Sysinternals Tools

The SysInternals Tools suite offers portable Windows applications for system administration, including Process Explorer, Process Monitor, TCPView, and PSExec. These tools facilitate tasks like monitoring file system, registry, and network activity, essential for penetration testing and system management.

```cmd
# Example usage of procdump.exe from Sysinternals Tools
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula
```

### Task Manager

Windows Task Manager provides insights into running processes, system performance, startup programs, logged-in users, and services. It can be accessed via various methods and offers tabs for detailed information and management options.

- **Processes tab**: Lists running applications and background processes with resource usage details.
- **Performance tab**: Displays graphs and data on CPU, memory, disk, network, and GPU usage.
- **Startup tab**: Shows applications configured to start at boot and their impact on startup.
- **Users tab**: Displays logged-in users and associated processes/resource usage.
- **Details tab**: Provides in-depth information about running applications, including PID and resource usage.
- **Services tab**: Lists installed services and allows service management directly from Task Manager.

## Process Explorer

Part of the Sysinternals suite, Process Explorer offers advanced features for managing processes, including handle and DLL information, parent-child process relationships, and troubleshooting tools. It enhances the functionality of Task Manager for in-depth process analysis and management.
