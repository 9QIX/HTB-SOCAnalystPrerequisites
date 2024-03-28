# Windows Security

Security is a critical topic in Windows operating systems. Windows systems have many moving parts that present a vast attack surface. Due to the many built-in applications, features, and layers of settings, Windows systems can be easily misconfigured, thus opening them up to attack even if they are fully patched.

It has many built-in features that can be abused and has suffered from a wide variety of critical vulnerabilities, resulting in widely used and very effective remote and local exploits.

Microsoft has improved upon Windows security over the years. As our world's interconnectedness continues to expand and attackers become more sophisticated, Microsoft has continued to add new features that can be used by systems administrators to harden systems and actively block and detect attempts at intrusion and misuse.

## Security Principles

Windows follows certain security principles. These are units in the system that can be authorized or authenticated for a particular action. These units include users, computers on the network, threads, or processes. The principles are designed to make it more difficult for attackers or malicious software to gain unauthorized access and exploit the system in an unintended way.

### Security Identifier (SID)

Each of the security principals on the system has a unique security identifier (SID). The system automatically generates SIDs. This means that even if, for example, we have two identical users on the system, Windows can distinguish the two and their rights based on their SIDs. SIDs are string values with different lengths, which are stored in the security database. These SIDs are added to the user's access token to identify all actions that the user is authorized to take.

A SID consists of the Identifier Authority and the Relative ID (RID). In an Active Directory (AD) domain environment, the SID also includes the domain SID.

```powershell
PS C:\htb> whoami /user

USER INFORMATION
----------------

User Name           SID
=================== =============================================
ws01\bob S-1-5-21-674899381-4069889467-2080702030-1002
```

The SID is broken down into this pattern.

```powershell
(SID)-(revision level)-(identifier-authority)-(subauthority1)-(subauthority2)-(etc)
```

Let's break down the SID piece by piece.

| Number                          | Meaning              | Description                                                                                                                                                                                        |
| ------------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S                               | SID                  | Identifies the string as a SID.                                                                                                                                                                    |
| 1                               | Revision Level       | To date, this has never changed and has always been 1.                                                                                                                                             |
| 5                               | Identifier-authority | A 48-bit string that identifies the authority (the computer or network) that created the SID.                                                                                                      |
| 21                              | Subauthority1        | This is a variable number that identifies the user's relation or group described by the SID to the authority that created it. It tells us in what order this authority created the user's account. |
| 674899381-4069889467-2080702030 | Subauthority2        | Tells us which computer (or domain) created the number.                                                                                                                                            |
| 1002                            | Subauthority3        | The RID that distinguishes one account from another. Tells us whether this user is a normal user, a guest, an administrator, or part of some other group.                                          |

### Security Accounts Manager (SAM) and Access Control Entries (ACE)

SAM grants rights to a network to execute specific processes.

The access rights themselves are managed by Access Control Entries (ACE) in Access Control Lists (ACL). The ACLs contain ACEs that define which users, groups, or processes have access to a file or to execute a process, for example.

The permissions to access a securable object are given by the security descriptor, classified into two types of ACLs: the Discretionary Access Control List (DACL) or System Access Control List (SACL). Every thread and process started or initiated by a user goes through an authorization process. An integral part of this process is access tokens, validated by the Local Security Authority (LSA). In addition to the SID, these access tokens contain other security-relevant information. Understanding these functionalities is an essential part of learning how to use and work around these security mechanisms during the privilege escalation phase.

### User Account Control (UAC)

User Account Control (UAC) is a security feature in Windows to prevent malware from running or manipulating processes that could damage the computer or its contents. There is the Admin Approval Mode in UAC, which is designed to prevent unwanted software from being installed without the administrator's knowledge or to prevent system-wide changes from being made. Surely you have already seen the consent prompt if you have installed a specific software, and your system has asked for confirmation if you want to have it installed. Since the installation requires administrator rights, a window pops up, asking you if you want to confirm the installation. With a standard user who has no rights for the installation, execution will be denied, or you will be asked for the administrator password. This consent prompt interrupts the execution of scripts or binaries that malware or attackers try to execute until the user enters the password or confirms execution. To understand how UAC works, we need to know how it is structured and how it works, and what triggers the consent prompt. The following diagram, adapted from the source here, illustrates how UAC works.

![UAC Diagram](/Images/image-32.png)

## Registry

The Registry is a hierarchical database in Windows critical for the operating system. It stores low-level settings for the Windows operating system and applications that choose to use it. It is divided into computer-specific and user-specific data. We can open the Registry Editor by typing regedit from the command line or Windows search bar.

![Registry Editor](/Images/image-33.png)

The tree-structure consists of main folders (root keys) in which subfolders (subkeys) with their entries/files (values) are located. There are 11 different types of values that can be entered in a subkey.

| Value                   | Type                                                                                                                                                       |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| REG_BINARY              | Binary data in any form.                                                                                                                                   |
| REG_DWORD               | A 32-bit number.                                                                                                                                           |
| REG_DWORD_LITTLE_ENDIAN | A 32-bit number in little-endian format.                                                                                                                   |
| REG_DWORD_BIG_ENDIAN    | A 32-bit number in big-endian format.                                                                                                                      |
| REG_EXPAND_SZ           | A null-terminated string that contains unexpanded references to environment variables (for example, "%PATH%").                                             |
| REG_LINK                | A null-terminated Unicode string containing the target path of a symbolic link created by calling the RegCreateKeyEx function with REG_OPTION_CREATE_LINK. |
| REG_MULTI_SZ            | A sequence of null-terminated strings, terminated by an empty string (\0).                                                                                 |
| REG_NONE                | No defined value type.                                                                                                                                     |
| REG_QWORD               | A 64-bit number.                                                                                                                                           |
| REG_QWORD_LITTLE_ENDIAN | A 64-bit number in little-endian format.                                                                                                                   |
| REG_SZ                  | A null-terminated string.                                                                                                                                  |

Each folder under Computer is a key. The root keys all start with HKEY. A key such as HKEY-LOCAL-MACHINE is abbreviated to HKLM. HKLM contains all settings that are relevant to the local system. This root key contains six subkeys like SAM, SECURITY, SYSTEM, SOFTWARE, HARDWARE, and BCD, loaded into memory at boot time (except HARDWARE which

is dynamically loaded).

![Root Keys](/Images/image-34.png)

The entire system registry is stored in several files on the operating system. You can find these under C:\Windows\System32\Config\.

```powershell
PS C:\htb> ls

    Directory: C:\Windows\system32\config

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         12/7/2019   4:14 AM                Journal
d-----         12/7/2019   4:14 AM                RegBack
d-----         12/7/2019   4:14 AM                systemprofile
d-----         8/12/2020   1:43 AM                TxR
-a----         8/13/2020   6:02 PM        1048576 BBI
-a----         6/25/2020   4:36 PM          28672 BCD-Template
-a----         8/30/2020  12:17 PM       33816576 COMPONENTS
-a----         8/13/2020   6:02 PM         524288 DEFAULT
-a----         8/26/2020   7:51 PM        4603904 DRIVERS
-a----         6/25/2020   3:37 PM          32768 ELAM
-a----         8/13/2020   6:02 PM          65536 SAM
-a----         8/13/2020   6:02 PM          65536 SECURITY
-a----         8/13/2020   6:02 PM       87818240 SOFTWARE
-a----         8/13/2020   6:02 PM       17039360 SYSTEM
```

The user-specific registry hive (HKCU) is stored in the user folder (i.e., C:\Users\<USERNAME>\Ntuser.dat).

```powershell
PS C:\htb> gci -Hidden

    Directory: C:\Users\bob

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--h--         6/25/2020   5:12 PM                AppData
d--hsl         6/25/2020   5:12 PM                Application Data
d--hsl         6/25/2020   5:12 PM                Cookies
d--hsl         6/25/2020   5:12 PM                Local Settings
d--h--         6/25/2020   5:12 PM                MicrosoftEdgeBackups
d--hsl         6/25/2020   5:12 PM                My Documents
d--hsl         6/25/2020   5:12 PM                NetHood
d--hsl         6/25/2020   5:12 PM                PrintHood
d--hsl         6/25/2020   5:12 PM                Recent
d--hsl         6/25/2020   5:12 PM                SendTo
d--hsl         6/25/2020   5:12 PM                Start Menu
d--hsl         6/25/2020   5:12 PM                Templates
-a-h--         8/13/2020   6:01 PM        2883584 NTUSER.DAT
-a-hs-         6/25/2020   5:12 PM         524288 ntuser.dat.LOG1
-a-hs-         6/25/2020   5:12 PM        1011712 ntuser.dat.LOG2
-a-hs-         8/17/2020   5:46 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.0.regtrans-ms
-a-hs-         8/17/2020  12:13 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.1.regtrans-ms
-a-hs-         8/17/2020  12:13 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.2.regtrans-ms
-a-hs-         8/17/2020   5:46 PM          65536 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.blf
-a-hs-         6/25/2020   5:15 PM          65536 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TM.blf
-a-hs-         6/25/2020   5:12 PM         524288 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TMContainer000000000
                                                  00000000001.regtrans-ms
-a-hs-         6/25/2020   5:12 PM         524288 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TMContainer000000000
                                                  00000000002.regtrans-ms
---hs-         6/25/2020   5:12 PM             20 ntuser.ini
```

### Run and RunOnce Registry Keys

There are also so-called registry hives, which contain a logical group of keys, subkeys, and values to support software and files loaded into memory when the operating system is started or a user logs in. These hives are useful for maintaining access to the system. These are called Run and RunOnce registry keys.

The Windows registry includes the following four keys:

- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce

Here is

an example of the `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run` key while logged in to a system.

```powershell
PS C:\htb> reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
    SecurityHealth    REG_EXPAND_SZ    %windir%\system32\SecurityHealthSystray.exe
    RTHDVCPL    REG_SZ    "C:\Program Files\Realtek\Audio\HDA\RtkNGUI64.exe" -s
    Greenshot    REG_SZ    C:\Program Files\Greenshot\Greenshot.exe
```

Here is an example of the `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run` showing applications running under the current user while logged in to a system.

```powershell
PS C:\htb> reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
    OneDrive    REG_SZ    "C:\Users\bob\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background
    OPENVPN-GUI    REG_SZ    C:\Program Files\OpenVPN\bin\openvpn-gui.exe
    Docker Desktop    REG_SZ    C:\Program Files\Docker\Docker\Docker Desktop.exe
```

### Application Whitelisting

An application whitelist is a list of approved software applications or executables allowed to be present and run on a system. The goal is to protect the environment from harmful malware and unapproved software that does not align with the specific business needs of an organization. Implementing an enforced whitelist can be a challenge, especially in a large network. An organization should implement a whitelist in audit mode initially to make sure that all necessary applications are whitelisted and not blocked by an error of omission, which can cause more problems than it fixes.

Blacklisting, in contrast, specifies a list of harmful or disallowed software/applications to block, and all others are allowed to run/be installed. Whitelisting is based on a "zero trust" principle in which all software/applications are deemed "bad" except for those specifically allowed. Maintaining a whitelist generally has less overhead as a system administrator will only need to specify what is allowed and not constantly update a "blacklist" with new malicious applications.

Whitelisting is recommended by organizations such as NIST, especially in high-security environments.

### AppLocker

AppLocker is Microsoft's application whitelisting solution and was first introduced in Windows 7. AppLocker gives system administrators control over which applications and files users can run. It gives granular control over executables, scripts, Windows installer files, DLLs, packaged apps, and packed app installers.

It allows for creating rules based on file attributes such as the publisher's name (which can be derived from the digital signature), product name, file name, and version. Rules can also be set up based on file paths and hashes. Rules can be applied to either security groups or individual users, based on the business need. AppLocker can be deployed in audit mode first to test the impact before enforcing all of the rules.

### Local Group Policy

Group Policy allows administrators to set, configure, and adjust a variety of settings. In a domain environment, group policies are pushed down from a Domain Controller onto all domain-joined machines that Group Policy objects (GPOs) are linked to. These settings can also be defined on individual machines using Local Group Policy.

Group Policy can be configured locally, in both domain environments and non-domain environments. Local Group Policy can be used to tweak certain graphical and network settings that are otherwise not accessible via the Control Panel. It can also be used to lock down an individual computer policy with stringent security settings, such as only allowing certain programs to be installed/run or enforcing strict user account password requirements.

We can open the Local Group Policy Editor by opening the Start menu and typing gpedit.msc. The editor is split into two categories under Local Computer Policy - Computer Configuration and User Configuration.

![Local Group Policy Editor](/Images/image-35.png)

For example, we can open the Local Computer Policy to enable Credential Guard by enabling the setting Turn On Virtualization Based Security. Credential Guard is a feature in Windows 10 that protects against credential theft attacks by isolating the operating system's LSA process.

![Credential Guard Setting](/Images/image-36.png)

We can also enable fine-tuned account auditing and configure AppLocker from the Local Group Policy Editor. It is worth exploring Local Group Policy and learning about the wide variety of ways it can be used to lock down a Windows system.

### Windows Defender Antivirus

Windows Defender Antivirus (Defender), formerly known as Windows Defender, is built-in antivirus that ships for free with Windows operating systems. It was first released as a downloadable anti-spyware tool for Windows XP and Server 2003. Defender started coming prepackaged as part of the operating system with Windows Vista/Server 2008. The program was renamed to Windows Defender Antivirus with the Windows 10 Creators Update.

Defender comes with several features such as real-time protection, which protects the device from known threats in real-time and cloud-delivered protection, which works in conjunction with automatic sample submission to upload suspicious files for analysis. When files are submitted to the cloud protection service, they are "locked" to prevent any potentially malicious behavior until the analysis is complete. Another feature is Tamper Protection, which prevents security settings from being changed through the Registry, PowerShell cmdlets, or group policy.

Windows Defender is managed from the Security Center, from which a variety of additional security features and settings can be enabled and managed.

![Windows Security Center](/Images/image-37.png)

Real-time protection settings can be tweaked to add files, folders, and memory areas to controlled folder access to prevent unauthorized changes. We can also add files or folders to an exclusion list, so they are not scanned. An example would be excluding a folder of tools used for penetration testing from scanning as they will be flagged malicious and quarantined or removed from the system. Controlled folder access is Defender's built-in Ransomware protection.

We can use the PowerShell cmdlet Get-MpComputerStatus to check which protection settings are enabled.

```powershell
PS C:\htb> Get-MpComputerStatus | findstr "True"
AMServiceEnabled                : True
AntispywareEnabled              : True
AntivirusEnabled                : True
BehaviorMonitorEnabled          : True
IoavProtectionEnabled           : True
IsTamperProtected               : True
NISEnabled                      : True
OnAccessProtectionEnabled       : True
RealTimeProtectionEnabled       : True
```

While no antivirus solution is perfect, Windows Defender does very well in monthly detection rate tests compared to other solutions, even paid ones. Also, since it comes preinstalled as part of the operating system, it does not introduce "bloat" to the system, such as other programs that add browser extensions and trackers. Other products are known to slow down the system due to the way they hook into the operating system.

Windows Defender is not without its flaws and should be part of a defense-in-depth strategy built around core principles of configuration and patch management, not treated as a silver bullet for protecting our systems. Definitions are updated constantly, and new versions of Windows Defender are built-in to major operating releases such as Windows

10, Windows 11, and Windows Server.

### Windows Firewall

The Windows Firewall is a security feature included in Windows XP and later versions of Windows operating systems. It's designed to monitor and restrict inbound and outbound network traffic based on predetermined security rules. The firewall is enabled by default in modern Windows operating systems.

Windows Firewall is a host-based firewall, meaning it runs on individual computers and can be configured through the Control Panel or via Group Policy in a domain environment.

![Windows Firewall Control Panel](/Images/image-38.png)

Windows Firewall with Advanced Security provides an interface for more advanced configuration options, such as creating inbound and outbound rules, configuring connection security rules, and monitoring active firewall rules and security associations.

```powershell
PS C:\htb> Get-NetFirewallProfile | select Name,Enabled

Name                      Enabled
----                      -------
DomainProfile             True
PrivateProfile            True
PublicProfile             True
```

This command displays the status of Windows Firewall profiles. We see that all three profiles (Domain, Private, Public) are enabled, which is the default configuration.

We can also use PowerShell to add rules to the firewall. Here is an example of adding a rule to allow inbound traffic on port 8080:

```powershell
New-NetFirewallRule -DisplayName "Allow Inbound Port 8080" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow
```

### PowerShell Security

PowerShell is a powerful scripting language built into Windows operating systems. While it's incredibly useful for system administrators and power users, it can also be a security risk if not properly secured.

Microsoft has implemented several security features in PowerShell to mitigate risks, such as script block logging, transcription logging, Constrained Language Mode, and Just Enough Administration (JEA).

Script block logging logs blocks of code as they are executed, while transcription logging logs all input and output as a script runs. These logs can be invaluable for forensics and incident response.

Constrained Language Mode restricts which PowerShell language elements and cmdlets can be used to limit what an attacker can do if they gain access to a system.

Just Enough Administration (JEA) is a PowerShell toolkit that allows for the delegation of specific tasks to users without giving them full administrative rights. This can help limit the impact of a compromised account.

PowerShell Execution Policy is a security feature that controls the conditions under which PowerShell loads configuration files and runs scripts. There are several levels of execution policy, ranging from unrestricted to restricted. The default policy is restricted, which prevents all scripts from running.

```powershell
PS C:\htb> Get-ExecutionPolicy

Restricted
```

We can change the execution policy to RemoteSigned, which allows local scripts to run but requires downloaded scripts to be signed by a trusted publisher:

```powershell
PS C:\htb> Set-ExecutionPolicy RemoteSigned
```

PowerShell's default logging functionality is disabled by default. To enable it, we can use Group Policy or configure it manually via the registry:

```powershell
Set-ItemProperty -Path 'HKLM:\Software\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging' -Name 'EnableScriptBlockLogging' -Value 1
```

In summary, while PowerShell is an incredibly powerful tool for both administrators and attackers, it's essential to understand and implement the security features built into PowerShell to mitigate risks.

### Conclusion

Windows security is a vast topic that requires continuous learning and adaptation due to the ever-evolving nature of cyber threats. Understanding the underlying principles of Windows security, such as SIDs, ACLs, and the Registry, is crucial for system administrators and security professionals. Additionally, familiarizing oneself with security features such as UAC, AppLocker, Windows Defender, Windows Firewall, and PowerShell security can help mitigate risks and protect Windows systems from various threats.

By implementing a defense-in-depth strategy that combines proactive measures such as patch management, application whitelisting, and robust security configurations with reactive measures such as antivirus software, firewalls, and intrusion detection systems, organizations can significantly improve their overall security posture and better defend against cyber threats.

## Questions

- Find the SID of the bob.smith user.

```powershell
wmic useraccount where "name='bob.smith'" get sid
```

This command is written for the WMIC (Windows Management Instrumentation Command-line) tool. Here's the breakdown:

- `wmic`: This is the command-line tool used to access and retrieve information from the Windows Management Instrumentation (WMI) database.
- `useraccount`: This specifies the WMI class to query, which represents user accounts on the system.
- `where "name='bob.smith'"`: This part filters the user accounts based on the name attribute. It specifies that only user accounts with the name "bob.smith" should be included in the results.
- `get sid`: This part specifies the property to retrieve from the filtered user accounts. It tells WMIC to retrieve the SID (Security Identifier) property of the selected user accounts.

When you run this WMIC command, it will query the WMI database to find the SID of the user account with the name "bob.smith" and display it in the output.

**_OR_**

```powershell
Get-WmiObject -Class Win32_UserAccount -filter “name = ‘bob.smith’”
```

This command is written in PowerShell. Here's the breakdown:

- `Get-WmiObject`: This is a cmdlet in PowerShell used to retrieve WMI objects.
- `-Class Win32_UserAccount`: This specifies the WMI class to query, which in this case is `Win32_UserAccount`, representing user accounts on the system.
- `-Filter "name = 'bob.smith'"`: This part filters the user accounts based on the name attribute. It specifies that only user accounts with the name "bob.smith" should be included in the results.

When you run this PowerShell command, it will query the WMI database to find user accounts with the name "bob.smith" and return information about those accounts.

```powershell
SID
S-1-5-21-2614195641-1726409526-3792725429-1003
```

- What 3rd party security application is disabled at startup for the current user? (The answer is case sensitive).
