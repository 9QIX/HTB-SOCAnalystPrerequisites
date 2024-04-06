# Working with the Windows Event Log

From a SOC Analyst or IT Administrator's perspective, monitoring, collecting, and categorizing events occurring on all machines across the network is an invaluable source of information for defenders proactively analyzing and protecting their network from suspicious activity. On the other hand, attackers can see this as an opportunity to gain insight into the target environment, disrupt the flow of information, and as a way to hide their tracks. As we will see in later modules, sometimes we can find juicy information such as credentials hiding in event logs as a target system that we compromise during a penetration test. Other times, enumerating event logs can help us understand the level of logging in the environment (are just the defaults in place, or has the target organization configured more granular logging?). In this section, we will discuss the following:

1. What is the Windows Event Log?
2. What information does it log, and where does it store this information?
3. Interacting with the Event Log via the `wevtutil` command line utility
4. Interacting with the Event Log using PowerShell cmdlets

## What is the Windows Event Log?

A clear understanding of event logging is crucial to success in infosec. To kickstart our journey into gaining a thorough understanding of the Windows Event Log, there are a few key concepts that we need to define before diving in. These concepts will become the base upon which everything else will be built. The first one that needs to be explained is an event definition. Simply put, an event is any action or occurrence that can be identified and classified by a system's hardware or software. Events can be generated or triggered through a variety of different ways including some of the following:

- **User-Generated Events**: Movement of a mouse, typing on a keyboard, other user-controlled peripherals, etc.
- **Application Generated Events**: Application updates, crashes, memory usage/consumption, etc.
- **System Generated Events**: System uptime, system updates, driver loading/unloading, user login, etc.

With so many events occurring at different intervals of time from various sources, how does a Windows system keep track of and categorize all of them? This is where our second key concept, known as event logging comes into play.

Event Logging as defined by Microsoft:

> "...provides a standard, centralized way for applications (and the operating system) to record important software and hardware events."

This definition sums up the question quite nicely. However, let us attempt to break it down a bit. As we discussed beforehand, there are a lot of events that are being triggered or generated concurrently on a system. Each event will have its own source that provides the information and details behind the event in its own format. So how does it handle all of this information?

Windows attempts to resolve this issue by providing a standardized approach to recording, storing, and managing events and event information through a service known as the Windows Event Log. As its name suggests, the Event Log manages events and event logs, however, in addition to this functionality it also opens up a special API that allows applications to maintain and manage their own separate logs.

### Event Log Categories and Types

The main four log categories include application, security, setup, and system. Another type of category also exists called forwarded events.

| Log Category     | Log Description                                                                                                                                                                                                     |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| System Log       | The system log contains events related to the Windows system and its components. A system-level event could be a service failing at startup.                                                                        |
| Security Log     | Self-explanatory; these include security-related events such as failed and successful logins, and file creation/deletion. These can be used to detect various types of attacks that we will cover in later modules. |
| Application Log  | This stores events related to any software/application installed on the system. For example, if Slack has trouble starting it will be recorded in this log.                                                         |
| Setup Log        | This log holds any events that are generated when the Windows operating system is installed. In a domain environment, events related to Active Directory will be recorded in this log on domain controller hosts.   |
| Forwarded Events | Logs that are forwarded from other hosts within the same network.                                                                                                                                                   |

There are five types of events that can be logged on Windows systems:

| Type of Event | Event Description                                                                                                                                                                                                                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Error         | Indicates a major problem, such as a service failing to load during startup, has occurred.                                                                                                                                                                                                                         |
| Warning       | A less significant log but one that may indicate a possible problem in the future. One example is low disk space. A Warning event will be logged to note that a problem may occur down the road. A Warning event is typically when an application can recover from the event without losing functionality or data. |
| Information   | Recorded upon the successful operation of an application, driver, or service, such as when a network driver loads successfully. Typically not every desktop application will log an event each time they start, as this could lead to a considerable amount of extra "noise" in the logs.                          |
| Success Audit | Recorded when an audited security access attempt is successful, such as when a user logs on to a system.                                                                                                                                                                                                           |
| Failure Audit | Recorded when an audited security access attempt fails, such as when a user attempts to log in but types their password in wrong. Many audit failure events could indicate an attack, such as Password Spraying.                                                                                                   |

Each log can have one of five severity levels associated with it, denoted by a number:

| Severity Level | Level # | Description                                                                                                                                                                                    |
| -------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Verbose        | 5       | Progress or success messages.                                                                                                                                                                  |
| Information    | 4       | An event that occurred on the system but did not cause any issues.                                                                                                                             |
| Warning        | 3       | A potential problem that a sysadmin should dig into.                                                                                                                                           |
| Error          | 2       | An issue related to the system or service that does not require immediate attention.                                                                                                           |
| Critical       | 1       | This indicates a significant issue related to an application or a system that requires urgent attention by a sysadmin that, if not addressed, could lead to system or application instability. |

### Elements of a Windows Event Log

The Windows Event Log provides information about hardware and software events on a Windows system. All event logs are stored in a standard format and include the following elements:

- **Log name**: As discussed above, the name of the event log where the events will be written. By default, events are logged for system, security, and applications.
- **Event date/time**: Date and time when the event occurred
- **Task Category**: The type of recorded event log
- **Event ID**: A unique identifier for sysadmins to identify a specific logged event
- **Source**: Where the log originated from, typically the name of a program or software application
- **Level**: Severity level of the event. This can be information, error, verbose, warning, critical
- **User**: Username of who logged onto the host when the event occurred
- **Computer**: Name of the computer where the event is logged

### Windows Event Log Technical Details

The Windows Event Log is handled by the EventLog services. On a Windows system, the service's display name is "Windows Event Log", and it runs inside the service host process `svchost.exe`. It is set to start automatically at system boot by default. It is difficult to stop the EventLog service as it has multiple dependency services. If it is stopped, it will likely cause significant system instability. By default, Windows Event Logs are stored in `C:\Windows\System32\winevt\logs` with the file extension `.evtx`.

```powershell
PS C:\htb> ls C:\Windows\System32\winevt\logs

    Directory: C:\Windows\System32\winevt\logs


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/16/2022   2:19 PM        7409664 Application.evtx
-a----         6/14/2022   8:20 PM          69632 HardwareEvents.evtx
-a----         6/14/2022   8:20 PM          69632 Internet Explorer.evtx
-a----         6/14/2022   8:20 PM          69632 Key Management Service.evtx
-a----         8/23/2022   7:01 PM          69632 Microsoft-Client-License-Flexible-P
                                                  latform%4Admin.evtx
-a----        11/16/2022   2:19 PM        1052672 Microsoft-Client-Licensing-Platform
                                                  %4Admin.evtx


<SNIP>
```

## Working with the Windows Event Log

We can interact with the Windows Event log using the Windows Event Viewer GUI application, the command line utility `wevtutil`, or using the `Get-WinEvent` PowerShell cmdlet. Both `wevtutil` and `Get-WinEvent` can be used to query Event Logs on both local and remote Windows systems via `cmd.exe` or PowerShell.

### Interacting with the Windows Event Log - `wevtutil`

The `wevtutil` command line utility can be used to retrieve information about event logs. It can also be used to export, archive, and clear logs, among other commands.

To enumerate the names of all logs present on a Windows system, we can use the `el` parameter:

```
C:\htb> wevtutil el
AMSI/Debug
AirSpaceChannel
Analytic
Application
DirectShowFilterGraph
DirectShowPluginControl
Els_Hyphenation/Analytic
EndpointMapper
FirstUXPerf-Analytic
ForwardedEvents
General Logging
HardwareEvents
```

To display configuration information for a specific log, such as whether the log is enabled or not, the maximum size, permissions, and where the log is stored on the system, we can use the `gl` parameter:

```
C:\htb> wevtutil gl "Windows PowerShell"
name: Windows PowerShell
enabled: true
type: Admin
owningPublisher:
isolation: Application
channelAccess: O:BAG:SYD:(A;;0x2;;;S-1-15-2-1)(A;;0x2;;;S-1-15-3-1024-3153509613-960666767-3724611135-2725662640-12138253-543910227-1950414635-4190290187)(A;;0xf0007;;;SY)(A;;0x7;;;BA)(A;;0x7;;;SO)(A;;0x3;;;IU)(A;;0x3;;;SU)(A;;0x3;;;S-1-5-3)(A;;0x3;;;S-1-5-33)(A;;0x1;;;S-1-5-32-573)
logging:
  logFileName: %SystemRoot%\System32\Winevt\Logs\Windows PowerShell.evtx
  retention: false
  autoBackup: false
  maxSize: 15728640
publishing:
  fileMax: 1
```

To query for events, we can use the `qe` parameter. For example, to display the last 5 most recent events from the Security log in text format:

```
C:\htb> wevtutil qe Security /c:5 /rd:true /f:text
```

We can also export events from a specific log for offline processing:

```
C:\htb> wevtutil epl System C:\system_export.evtx
```

### Interacting with the Windows Event Log - PowerShell

Similarly, we can interact with Windows Event Logs using the `Get-WinEvent` PowerShell cmdlet. Like with the `wevtutil` examples, some commands require local admin-level access.

To list all logs on the computer, giving us the number of records in each log:

```powershell
PS C:\htb> Get-WinEvent -ListLog *
```

To get details about a specific log, such as the size of the Security log:

```powershell
PS C:\htb> Get-WinEvent -ListLog Security
```

To query for the last 5 events recorded in the Security log:

```powershell
PS C:\htb> Get-WinEvent -LogName 'Security' -MaxEvents 5 | Select-Object -ExpandProperty Message
```

To filter for logon failures in the Security log, checking for Event ID 4625: "An account failed to log on":

```powershell
PS C:\htb> Get-WinEvent -FilterHashTable @{LogName='Security';ID='4625 '}
```

To check all System logs for only critical events with information level 1:

```powershell
PS C:\htb> Get-WinEvent -FilterHashTable @{LogName='System';Level='1'} | select-object -ExpandProperty Message
```

Practice more with `wevtutil` and `Get-WinEvent` to become more comfortable with searching logs. Microsoft provides some examples for `Get-WinEvent`, while [this site](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil) shows examples for `wevtutil`, and [this site](https://www.thomasmaurer.ch/2019/02/15-useful-powershell-get-winevent-examples/) has some additional examples for using `Get-WinEvent`.

## Moving On

This section introduced the Windows Event Log, a vast topic that we will dig much deeper into in later modules. Try out the various examples in this section and get comfortable using both tools to query for specific information. In later modules, we will see how we can sometimes find sensitive data, such as passwords, in Event Logs. Logging on Windows is very powerful when configured properly. Each system generates a massive amount of logs, and, as we saw with all the possible Event IDs, we can get quite granular with what exactly we choose to log. All of this data on its own would be very difficult to constantly query and is most effective when forwarded to a SIEM tool that can be used to set up alerts on specific Event IDs which may be indicative of an attack, such as Kerberoasting, Password Spraying, or other less common attacks. As penetration testers, we should be familiar with Windows Event Log, how we can use it to gain information about the environment, and sometimes even extract sensitive data. For blue teamers, in-depth knowledge of Windows Event Log and how to leverage it for effective alerting and monitoring is critical.

In the next section, we will cover working with networking operations from the command line on a Windows system.
