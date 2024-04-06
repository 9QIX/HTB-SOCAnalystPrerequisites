Here is the document converted to Markdown format:

# Working with Services

In our previous section, we discussed filtering and the pipeline using an example of finding services on the host from the eyes of a pentester. In this section, we will dive deeper into this and flip it on its head. We are going to look at it from the eyes of an administrator.

## Scenario

Mr. Tanaka messaged the Helpdesk stating that he noticed a window pop up earlier in the day and thought it was just Windows updates running, as lots of information flashed by in the window. However, now he reports that alerts stating that Defender is turned off also popped up, and his host is acting sluggish. We need to look into this, determine what services related to Defender are shut off, and enable them again if we can. Later we will look into the event logs and see what happened.

Service administration is crucial in managing hosts and ensuring our security posture remains unchanged. This section will cover how to query, start, stop, and edit services and their permissions as needed. We will also discuss ways to interact with them locally and remotely. It is time to dive in and acquire our next CLI Kung-Fu Belt.

## What Are Services and How Do We Interact with Them Using Powershell?

Services in the Windows Operating system at their core are singular instances of a component running in the background that manages and maintains processes and other needed components for applications used on the host. Services usually do not require interaction from the user and have no tangible interface for them to interact with. They also exist as a singular instance of the service on the host, while a service can maintain multiple instances of a process. A process can be considered a temporary container for a user or application to perform tasks. Windows has three categories of services: Local Services, Network Services, and System Services. Many different services (including the core components within the Windows operating system) handle multiple instances of processes simultaneously. PowerShell provides us with the module Microsoft.PowerShell.Management, which contains several cmdlets for interacting with Services. As with everything in PowerShell, if you are unsure where to start or what cmdlet you need, take advantage of the built-in help to get you started.

### Getting Help (Services)

```powershell
PS C:\htb> Get-Help *-Service

Name                              Category  Module                    Synopsis
----                              --------  ------                    --------
Get-Service                       Cmdlet    Microsoft.PowerShell.Man… …
New-Service                       Cmdlet    Microsoft.PowerShell.Man… …
Remove-Service                    Cmdlet    Microsoft.PowerShell.Man… …
Restart-Service                   Cmdlet    Microsoft.PowerShell.Man… …
Resume-Service                    Cmdlet    Microsoft.PowerShell.Man… …
Set-Service                       Cmdlet    Microsoft.PowerShell.Man… …
Start-Service                     Cmdlet    Microsoft.PowerShell.Man… …
Stop-Service                      Cmdlet    Microsoft.PowerShell.Man… …
Suspend-Service                   Cmdlet    Microsoft.PowerShell.Man… …
```

Now, let us start our triage of Mr. Tanaka's host and see what is going on.

> Note: Keep in mind that to manage or modify services outside of running queries, we will need to have the correct permissions to do so. This means our user should ideally be a local administrator on the host or have been given the permissions from the domain groups they are a member of. Opening PowerShell in an administrative context would also work.

### Investigating Running Services

We first need to get a quick running list of services from our target host. Services can have a status set as Running, Stopped, or Paused and can be set up to start manually (user interaction), automatically (at system startup), or on a delay after system boot. Users with administrative privileges can usually create, modify, and delete services. Misconfigurations around service permissions are a common privilege escalation vector on Windows systems.

```powershell
PS C:\htb> Get-Service | ft DisplayName,Status

DisplayName                                                                         Status
-----------                                                                         ------

Adobe Acrobat Update Service                                                       Running
OpenVPN Agent agent_ovpnconnect                                                    Running
Adobe Genuine Monitor Service                                                      Running
Adobe Genuine Software Integrity Service                                           Running
Application Layer Gateway Service                                                  Stopped
Application Identity                                                               Stopped
Application Information                                                            Running
Application Management                                                             Stopped
App Readiness                                                                      Stopped
Microsoft App-V Client                                                             Stopped
AppX Deployment Service (AppXSVC)                                                  Running
AssignedAccessManager Service                                                      Stopped
Windows Audio Endpoint Builder                                                     Running
Windows Audio                                                                      Running
ActiveX Installer (AxInstSV)                                                       Stopped
GameDVR and Broadcast User Service_172433                                          Stopped
BitLocker Drive Encryption Service                                                 Running
Base Filtering Engine                                                              Running
<SNIP>

PS C:\htb> Get-Service | measure

Count             : 321
```

To make it a little clearer to run, we piped our service listing into format-table and chose the properties DisplayName and Status to display in our console. On the second command issued, we measured the number of services that appear in the listing just to get a sense of how many we are working with. 321 services are a lot to scroll through and work with at once, so we need to pare it down a bit more. From Mr. Tanaka's request, he mentioned a potential issue with Windows Defender, so let us filter out any services not related to that.

### Precision Look at Defender

```powershell
PS C:\htb> Get-Service | where DisplayName -like '*Defender*' | ft DisplayName,ServiceName,Status

DisplayName                                             ServiceName  Status
-----------                                             -----------  ------
Windows Defender Firewall                               mpssvc      Running
Windows Defender Advanced Threat Protection Service     Sense       Stopped
Microsoft Defender Antivirus Network Inspection Service WdNisSvc    Running
Microsoft Defender Antivirus Service                    WinDefend   Stopped
```

Now we can see just the services related to Defender, and we can see that for some reason, the Microsoft Defender Antivirus Service (WinDefend) is indeed turned off. For now, to ensure the protection of Mr. Tanaka's host, let us try and turn it back on using the `Start-Service` cmdlet.

### Resume / Start / Restart a Service

```powershell
PS C:\htb> Start-Service WinDefend
```

As we ran the cmdlet `Start-Service` as long as we did not get an error message like "ParserError: This script contains malicious content and has been blocked by your antivirus software." or others, the command executed successfully. We can check again by querying the service.

### Checking Our Work

```powershell
PS C:\htb>  get-service WinDefend

Status   Name               DisplayName
------   ----               -----------
Running  WinDefend          Microsoft Defender Antivirus Service
```

Notice how we utilized the service Name to start and query the service instead of anything in the DisplayName. For now, Defender is back up and running, so the first mission is accomplished. While here, let us look around and see what else is happening. As we look through the services a bit more to see what is there, we notice a Service with an odd DisplayName.

```powershell
PS C:\htb> get-service

Stopped  SmsRouter          Microsoft Windows SMS Router Service.
Stopped  SNMPTrap           SNMP Trap
Stopped  spectrum           Windows Perception Service
Running  Spooler            Totally still used for Print Spooli...
Stopped  sppsvc             Software Protection
Running  SSDPSRV            SSDP Discovery
```

We cannot find any information on this particular service, and its DisplayName having changed is odd, so to be safe, we will stop the service for now and let one of our team members on the security team investigate it.

### Stopping a Service

```powershell
PS C:\htb> Stop-Service Spooler

PS C:\htb> Get-Service Spooler

Status   Name               DisplayName
------   ----               -----------
Stopped  spooler            Totally still used for Print Spooli...
```

Now we can see that using the `Stop-Service`, we stopped the operating status of the Spooler service. Now that we have stopped the service let us set the startup type of the service now from Automatic to Disabled until further investigation can be taken.

### Set-Service

```powershell
PS C:\htb> get-service spooler | Select-Object -Property Name, StartType, Status, DisplayName

Name    StartType  Status DisplayName
----    ---------  ------ -----------
spooler Automatic Stopped Totally still used for Print Spooling...


PS C:\htb> Set-Service -Name Spooler -StartType Disabled

PS C:\htb> Get-Service -Name Spooler | Select-Object -Property StartType

StartType
---------
 Disabled
```

Ok, now our Spooler service has been stopped, and its Startup changed to Disabled for now. Modifying a running service is reasonably straightforward. Ensure that if you attempt to make any modifications, you are an Administrator for the host or on the domain. Removing services in PowerShell is difficult right now. The cmdlet `Remove-Service` only works if you are using PowerShell version 7. By default, our hosts will open and run PowerShell version 5.1. For now, if you wish to remove a service and its entries, use the `sc.exe` tool.

## How Do We Interact with Remote Services using PowerShell?

Now that we know how to work with services, let us look at how we can interact with remote hosts. Since Mr. Tanaka's host is in a domain, we can easily query and check the running services on other hosts. The `-ComputerName` parameter allows us to specify that we want to query a remote host.

### Remotely Query Services

```powershell
PS C:\htb> get-service -ComputerName ACADEMY-ICL-DC

Status   Name               DisplayName
------   ----               -----------
Running  ADWS               Active Directory Web Services
Stopped  AppIDSvc           Application Identity
Stopped  AppMgmt            Application Management
Stopped  AppReadiness       App Readiness
Stopped  AppXSvc            AppX Deployment Service (AppXSVC)
Running  BFE                Base Filtering Engine
Stopped  BITS               Background Intelligent Transfer Ser...
<SNIP>
```

### Filtering our Output

```powershell
PS C:\htb> Get-Service -ComputerName ACADEMY-ICL-DC | Where-Object {$_.Status -eq "Running"}

Status   Name               DisplayName
------   ----               -----------
Running  ADWS               Active Directory Web Services
Running  BFE                Base Filtering Engine
Running  COMSysApp          COM+ System Application
Running  CoreMessagingRe... CoreMessaging
Running  CryptSvc           Cryptographic Services
Running  DcomLaunch         DCOM Server Process Launcher
Running  Dfs                DFS Namespace
Running  DFSR               DFS Replication
```

One interesting thing of note here is that since PowerShell handles everything as an object, even the output from a remote command, we can use the PowerShell pipeline to dissect an object's properties with `Where-Object`. Our results returned only the services with a status when it was run of Running. We can use these combinations for any number of things. One great example would be to query our hosts for a specific property, such as if the status was Running, if a DisplayName is set to something specific, etc. Regarding remote interactions, we can also use the `Invoke-Command` cmdlet. Let us try and query multiple hosts and see the status of the UserManager service.

### Invoke-Command

```powershell
PS C:\htb> invoke-command -ComputerName ACADEMY-ICL-DC,LOCALHOST -ScriptBlock {Get-Service -Name 'windefend'}

Status   Name               DisplayName                            PSComputerName
------   ----               -----------                            --------------
Running  windefend          Microsoft Defender Antivirus Service   LOCALHOST
Running  windefend          Windows Defender Antivirus Service     ACADEMY-ICL-DC
```

Let us break this down now:

- `Invoke-Command`: We are telling PowerShell that we want to run a command on a local or remote computer.
- `Computername`: We provide a comma-defined list of computer names to query.
- `ScriptBlock {commands to run}`: This portion is the enclosed command we want to run on the computer. For it to run, we need it to be enclosed in {}.

Interacting with hosts in this manner can expedite much of our work.

**Scenario**: Earlier in this section, we saw a service (Spooler) that had a DisplayName that was modified. This could potentially clue us in on an issue within our environment. Using the `-ComputerName` parameter or the `Invoke-Command` cmdlet to query all of the hosts within our environment and check the DisplayName properties to see if any other host has been affected. As an administrator, having access to this kind of power is invaluable and can often help reduce the time a threat is on the host and help get ahead of the issue and work to kick the threat out.

Understanding services and managing them on a host is essential from an admin and pentester perspective. We can do a lot with them, including anything from privilege escalation, to persistence and more. Moving on to our next section, we will introduce the Windows Registry and how to interact with it on a Windows host.
