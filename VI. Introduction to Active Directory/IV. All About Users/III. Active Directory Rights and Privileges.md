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

## User Rights Assignment

Depending on their group membership and other factors such as privileges assigned via Group Policy (GPO), users can have various rights assigned to their account. This [Microsoft article on User Rights Assignment](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/user-rights-assignment) provides a detailed explanation of each of the user rights that can be set in Windows. Not every right listed here is important to us from a security standpoint as penetration testers or defenders, but some rights granted to an account can lead to unintended consequences such as privilege escalation or access to sensitive files.

For example, let's say we can gain write access over a Group Policy Object (GPO) applied to an OU containing one or more users that we control. In this example, we could potentially leverage a tool such as SharpGPOAbuse to assign targeted rights to a user. We may perform many actions in the domain to further our access with these new rights. A few examples include:

- **SeRemoteInteractiveLogonRight**: Allows the user to log onto a host via Remote Desktop (RDP), potentially accessing sensitive data or escalating privileges.
- **SeBackupPrivilege**: Grants the ability to create system backups, obtain copies of sensitive system files like SAM and NTDS.dit, which contain passwords.
- **SeDebugPrivilege**: Allows debugging and adjusting memory of a process, potentially retrieving stored credentials.
- **SeImpersonatePrivilege**: Allows impersonation of privileged accounts, aiding in privilege escalation.
- **SeLoadDriverPrivilege**: Enables loading and unloading of device drivers, which can be used to escalate privileges or compromise a system.
- **SeTakeOwnershipPrivilege**: Allows taking ownership of objects, providing access to otherwise restricted resources.

There are many techniques available to abuse user rights, and it's essential to understand the impact that assigning the wrong privilege to an account can have within Active Directory. A small admin mistake can lead to a complete system or enterprise compromise.

## Viewing a User's Privileges

After logging into a host, typing the command `whoami /priv` will give a listing of all user rights assigned to the current user. Some rights are only available to administrative users and can only be listed/leveraged when running an elevated CMD or PowerShell session. These concepts of elevated rights and User Account Control (UAC) are security features introduced with Windows Vista that default to restricting applications from running with full permissions unless absolutely necessary. If we compare and contrast the rights available to us as an admin in a non-elevated console vs. an elevated console, we will see that they differ drastically.

### Standard Domain User's Rights

```plaintext
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

### Domain Admin Rights Non-Elevated

```plaintext
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

```plaintext
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

```plaintext
PS C:\htb> whoami /priv

PRIVILEGES INFORMATION
----------------------
SeShutdownPrivilege           Shut down the system           Disabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Disabled
```

As attackers and defenders, we need to understand the rights granted to users via membership from built-in security groups in Active Directory. It's not uncommon to find seemingly low privileged users added to one or more of these groups, which can be used to further access or compromise the domain. Access to these groups should be strictly controlled. It is typically best practice to leave most of these groups empty and only add an account to a group if a one-off action needs to be performed or a repetitive task needs to be set up. Any accounts added to one of the groups discussed in this section or granted extra privileges should be strictly controlled and monitored, assigned a very strong password or passphrase, and should be separate from an account used by a sysadmin to perform their day-to-day duties.

Now that we've begun to touch on some security considerations in AD related to user privileges and built-in group membership let's walk through some critical points for securing an Active Directory installation.

```

```
