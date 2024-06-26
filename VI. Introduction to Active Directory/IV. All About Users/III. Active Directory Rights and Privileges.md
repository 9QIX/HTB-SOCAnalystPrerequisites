# Active Directory Rights and Privileges

Rights and privileges are the cornerstones of AD management and, if mismanaged, can easily lead to abuse by attackers or penetration testers. Access rights and privileges are two important topics in AD (and infosec in general), and we must understand the difference.

- **Rights** are permissions to access an object such as a file.
- **Privileges** grant a user permission to perform an action such as running a program, shutting down a system, resetting passwords, etc.

Privileges can be assigned individually to users or conferred upon them via built-in or custom group membership. Windows computers have a concept called User Rights Assignment, which, while referred to as rights, are actually types of privileges granted to a user. We will discuss these later in this section. It's crucial to grasp the differences between rights and privileges and precisely how they apply to an AD environment.

## Built-in AD Groups

AD contains many default or built-in security groups, some of which grant their members powerful rights and privileges which can be abused to escalate privileges within a domain and ultimately gain Domain Admin or SYSTEM privileges on a Domain Controller (DC). Membership in many of these groups should be tightly managed as excessive group membership/privileges are a common flaw in many AD networks that attackers look to abuse. Some of the most common built-in groups are listed below:

| Group Name                         | Description                                                                                                                                                     |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Account Operators                  | Members can create and modify most types of accounts, including those of users, local groups, and global groups. They can log in locally to domain controllers. |
| Administrators                     | Members have full and unrestricted access to a computer or an entire domain if they are in this group on a Domain Controller.                                   |
| Backup Operators                   | Members can back up and restore all files on a computer, log on to domain controllers locally, and make shadow copies of the SAM/NTDS database.                 |
| Domain Admins                      | Members have full access to administer the domain and are members of the local administrator's group on all domain-joined machines.                             |
| Domain Computers                   | Contains all non-domain controller computers in the domain.                                                                                                     |
| Domain Controllers                 | Contains all DCs within a domain. New DCs are added to this group automatically.                                                                                |
| Domain Guests                      | Includes the domain's built-in Guest account. Members have a domain profile created when signing onto a domain-joined computer as a local guest.                |
| Domain Users                       | Contains all user accounts in a domain. A new user account created in the domain is automatically added to this group.                                          |
| Enterprise Admins                  | Membership in this group provides complete configuration access within the domain. Only exists in the root domain of an AD forest.                              |
| Event Log Readers                  | Members can read event logs on local computers. Created when a host is promoted to a domain controller.                                                         |
| Group Policy Creator Owners        | Members create, edit, or delete Group Policy Objects in the domain.                                                                                             |
| Hyper-V Administrators             | Members have complete and unrestricted access to all features in Hyper-V.                                                                                       |
| IIS_IUSRS                          | Built-in group used by Internet Information Services (IIS).                                                                                                     |
| Pre-Windows 2000 Compatible Access | Exists for backward compatibility with Windows NT 4.0 and earlier.                                                                                              |
| Print Operators                    | Members can manage, create, share, and delete printers connected to domain controllers.                                                                         |
| Protected Users                    | Members of this group are provided additional protections against credential theft and Kerberos abuse.                                                          |
| Read-only Domain Controllers       | Contains all Read-only domain controllers in the domain.                                                                                                        |
| Remote Desktop Users               | Used to grant users and groups permission to connect to a host via Remote Desktop (RDP).                                                                        |
| Remote Management Users            | Used to grant users remote access to computers via Windows Remote Management (WinRM).                                                                           |
| Schema Admins                      | Members can modify the Active Directory schema. Only exists in the root domain of an AD forest.                                                                 |
| Server Operators                   | Exists only on domain controllers. Members can modify services, access SMB shares, and backup files on DCs.                                                     |

Below we have provided some output regarding domain admins and server operators.

## Server Operators Group Details

```powershell
PS C:\htb>  Get-ADGroup -Identity "Server Operators" -Properties *

adminCount                      : 1
CanonicalName                   : INLANEFREIGHT.LOCAL/Builtin/Server Operators
CN                              : Server Operators
Created                         : 10/27/2021 8:14:34 AM
createTimeStamp                 : 10/27/2021 8:14:34 AM
Deleted                         :
Description                     : Members can administer domain servers
DisplayName                     :
DistinguishedName               : CN=Server Operators,CN=Builtin,DC=INLANEFREIGHT,DC=LOCAL
dSCorePropagationData           : {10/28/2021 1:47:52 PM, 10/28/2021 1:44:12 PM, 10/28/2021 1:44:11 PM, 10/27/2021
                                  8:50:25 AM...}
GroupCategory                   : Security
GroupScope                      : DomainLocal
groupType                       : -2147483643
HomePage                        :
instanceType                    : 4
isCriticalSystemObject          : True
isDeleted                       :
LastKnownParent                 :
ManagedBy                       :
MemberOf                        : {}
Members                         : {}
Modified                        : 10/28/2021 1:47:52 PM
modifyTimeStamp                 : 10/28/2021 1:47:52 PM
Name                            : Server Operators
nTSecurityDescriptor            : System.DirectoryServices.ActiveDirectorySecurity
ObjectCategory                  : CN=Group,CN=Schema,CN=Configuration,DC=INLANEFREIGHT,DC=LOCAL
ObjectClass                     : group
ObjectGUID                      : 0887487b-7b07-4d85-82aa-40d25526ec17
objectSid                       : S-1-5-32-549
ProtectedFromAccidentalDeletion : False
SamAccountName                  : Server Operators
sAMAccountType                  : 536870912
sDRightsEffective               : 0
SID                             : S-1-5-32-549
SIDHistory                      : {}
systemFlags                     : -1946157056
uSNChanged                      : 228556
uSNCreated                      : 12360
whenChanged                     : 10/28/2021 1:47:52 PM
whenCreated                     : 10/27/2021 8:14:34 AM
```

As we can see above, the default state of the Server Operators group is to have no members and is a domain local group by default. In contrast, the Domain Admins group seen below has several members and service accounts assigned to it. Domain Admins are also Global groups instead of domain local. More on group membership can be found later in this module. Be wary of who, if anyone, you give access to these groups. An attacker could easily gain the keys to the enterprise if they gain access to a user assigned to these groups.

## Domain Admins Group Membership

```powershell
PS C:\htb>  Get-ADGroup -Identity "Domain Admins" -Properties * | select DistinguishedName,GroupCategory,GroupScope,Name,Members

DistinguishedName : CN=Domain Admins,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
GroupCategory     : Security
GroupScope        : Global
Name              : Domain Admins
Members           : {CN=htb-student_adm,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=sharepoint
                    admin,CN=Users,DC=INLANEFREIGHT,DC=LOCAL, CN=FREIGHTLOGISTICSUSER,OU=Service
                    Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL, CN=PROXYAGENT,OU=Service
                    Accounts,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL...}
```

## User Rights Assignment

Depending on their group membership and other factors such as privileges assigned via Group Policy (GPO), users can have various rights assigned to their account. This [Microsoft article on User Rights Assignment](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment) provides a detailed explanation of each of the user rights that can be set in Windows. Not every right listed here is important to us from a security standpoint as penetration testers or defenders, but some rights granted to an account can lead to unintended consequences such as privilege escalation or access to sensitive files.

For example, let's say we can gain write access over a Group Policy Object (GPO) applied to an OU containing one or more users that we control. In this example, we could potentially leverage a tool such as SharpGPOAbuse to assign targeted rights to a user. We may perform many actions in the domain to further our access with these new rights. A few examples include:

| Privilege                     | Description                                                                                                                                                                                                                                              |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SeRemoteInteractiveLogonRight | This privilege could give our target user the right to log onto a host via Remote Desktop (RDP), which could potentially be used to obtain sensitive data or escalate privileges.                                                                        |
| SeBackupPrivilege             | This grants a user the ability to create system backups and could be used to obtain copies of sensitive system files that can be used to retrieve passwords such as the SAM and SYSTEM Registry hives and the NTDS.dit Active Directory database file.   |
| SeDebugPrivilege              | This allows a user to debug and adjust the memory of a process. With this privilege, attackers could utilize a tool such as Mimikatz to read the memory space of the Local System Authority (LSASS) process and obtain any credentials stored in memory. |
| SeImpersonatePrivilege        | This privilege allows us to impersonate a token of a privileged account such as NT AUTHORITY\SYSTEM. This could be leveraged with a tool such as JuicyPotato, RogueWinRM, PrintSpoofer, etc., to escalate privileges on a target system.                 |
| SeLoadDriverPrivilege         | A user with this privilege can load and unload device drivers that could potentially be used to escalate privileges or compromise a system.                                                                                                              |
| SeTakeOwnershipPrivilege      | This allows a process to take ownership of an object. At its most basic level, we could use this privilege to gain access to a file share or a file on a share that was otherwise not accessible to us.                                                  |

There are many techniques available to abuse user rights, and it's essential to understand the impact that assigning the wrong privilege to an account can have within Active Directory. A small admin mistake can lead to a complete system or enterprise compromise.

## Viewing a User's Privileges

After logging into a host, typing the command `whoami /priv` will give a listing of all user rights assigned to the current user. Some rights are only available to administrative users and can only be listed/leveraged when running an elevated CMD or PowerShell session. These concepts of elevated rights and User Account Control (UAC) are security features introduced with Windows Vista that default to restricting applications from running with full permissions unless absolutely necessary. If we compare and contrast the rights available to us as an admin in a non-elevated console vs. an elevated console, we will see that they differ drastically.

### Standard Domain User's Rights

```powershell
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== ========
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set DisabledetPrivilege Increase a process working set Disabled
```

### Domain Admin Rights Non-Elevated

```powershell
PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                          State
============================= ==================================== ========
SeShutdownPrivilege           Shut down the system                 Disabled
SeChangeNotifyPrivilege       Bypass traverse checking             Enabled
SeUndockPrivilege             Remove computer from docking station Disabled
SeIncreaseWorkingSetPrivilege Increase a process working set       Disabled
SeTimeZonePrivilege           Change the time zone                 Disabled
```

### Domain Admin Rights Elevated

```powershell
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State
========================================= ================================================================== ========
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeMachineAccountPrivilege                 Add workstations to domain                                         Disabled
SeSecurityPrivilege                       Manage auditing and security log                                   Disabled
SeTakeOwnershipPrivilege                  Take ownership of files or other objects                           Disabled
SeLoadDriverPrivilege                     Load and unload device drivers                                     Disabled
SeSystemProfilePrivilege                  Profile system performance                                         Disabled
SeSystemtimePrivilege                     Change the system time                                             Disabled
SeProfileSingleProcessPrivilege           Profile single process                                             Disabled
SeIncreaseBasePriorityPrivilege           Increase scheduling priority                                       Disabled
SeCreatePagefilePrivilege                 Create a pagefile                                                  Disabled
SeBackupPrivilege                         Back up files and directories                                      Disabled
SeRestorePrivilege                        Restore files and directories                                      Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeDebugPrivilege                          Debug programs                                                     Enabled
SeSystemEnvironmentPrivilege              Modify firmware environment values                                 Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled
SeRemoteShutdownPrivilege                 Force shutdown from a remote system                                Disabled
SeUndockPrivilege                         Remove computer from docking station                               Disabled
SeEnableDelegationPrivilege               Enable computer and user accounts to be trusted for delegation     Disabled
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Disabled
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled
SeCreateGlobalPrivilege                   Create global objects                                              Enabled
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeTimeZonePrivilege                       Change the time zone                                               Disabled
SeCreateSymbolicLinkPrivilege             Create symbolic links                                              Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Disabled
```

User rights increase based on the groups they are placed in or their assigned privileges. Below is an example of the rights granted to a member of the Backup Operators group:

### Backup Operator Rights

```powershell
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

As attackers and defenders, we need to understand the rights granted to users via membership from built-in security groups in Active Directory. It's not uncommon to find seemingly low privileged users added to one or more of these groups, which can be used to further access or compromise the domain. Access to these groups should be strictly controlled. It is typically best practice to leave most of these groups empty and only add an account to a group if a one-off action needs to be performed or a repetitive task needs to be set up. Any accounts added to one of the groups discussed in this section or granted extra privileges should be strictly controlled and monitored, assigned a very strong password or passphrase, and should be separate from an account used by a sysadmin to perform their day-to-day duties.

Now that we've begun to touch on some security considerations in AD related to user privileges and built-in group membership let's walk through some critical points for securing an Active Directory installation.
