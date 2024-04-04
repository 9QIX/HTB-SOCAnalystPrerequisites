# User and Group Management

As a system administrator, user and group management is a key skill as our users are often our main asset to manage and, usually, an organization's largest attack vector. As pentesters, understanding how to enumerate, interpret, and take advantage of users and groups is one of the easiest ways to gain access and elevate our privileges during a pentest engagement. This section will cover what users and groups are, how to manage them with PowerShell, and briefly introduce the concept of Active Directory domains and domain users.

## What are User Accounts?

User accounts are a way for personnel to access and use a host's resources. In certain circumstances, the system will also utilize a specially provisioned user account to perform actions. When thinking about accounts, we typically run into four different types:

- Service Accounts
- Built-in accounts
- Local users
- Domain users

### Default Local User Accounts

Several accounts are created in every instance of Windows as the OS is installed to help with host management and basic usage. Below is a list of the standard built-in accounts.

| Built-In Accounts   |                                                                                                                                                                  |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Account             | Description                                                                                                                                                      |
| Administrator       | This account is used to accomplish administrative tasks on the local host.                                                                                       |
| Default Account     | The default account is used by the system for running multi-user auth apps like the Xbox utility.                                                                |
| Guest Account       | This account is a limited rights account that allows users without a normal user account to access the host. It is disabled by default and should stay that way. |
| WDAGUtility Account | This account is in place for the Defender Application Guard, which can sandbox application sessions.                                                             |

## Brief Intro to Active Directory

In a nutshell, Active Directory (AD) is a directory service for Windows environments that provides a central point of management for users, computers, groups, network devices, file shares, group policies, devices, and trusts with other organizations. Think of it as the gatekeeper for an enterprise environment. Anyone who is a part of the domain can access resources freely, while anyone who is not is denied access to those same resources or, at a minimum, stuck waiting in the visitors center.

Within this section, we care about AD in the context of users and groups. We can administer them from PowerShell on any domain joined host utilizing the ActiveDirectory Module. Taking a deep dive into Active Directory would take more than one section, so we will not try here. To learn more about AD, you should check out the Introduction to Active Directory module.

### Local vs. Domain Joined Users

How are they different?

Domain users differ from local users in that they are granted rights from the domain to access resources such as file servers, printers, intranet hosts, and other objects based on user and group membership. Domain user accounts can log in to any host in the domain, while the local user only has permission to access the specific host they were created on.

It is worth looking through the documentation on accounts to understand better how the various accounts work together on an individual Windows system and across a domain network. Take some time to look them over and understand the nuances between them. Understanding their uses and the utility of each type of account can make or break a pentesters attempt at privileged access or lateral movement during a penetration test.

## What Are User Groups?

Groups are a way to sort user accounts logically and, in doing so, provide granular permissions and access to resources without having to manage each user manually. For example, we could restrict access to a specific directory or share so that only users who need access can view the files. On a singular host, this does not mean much to us. However, logical grouping is essential to maintain a proper security posture within a domain of hundreds, if not thousands, of users. From a domain perspective, we have several different types of groups that can hold not only users but end devices like PCs, printers, and even other groups. This concept is too deep of a dive for this module. However, we will talk about how to manage groups for now. If you wish to know more and get a deep dive into Active Directory and how it utilizes groups to maintain security, check out this module.

```powershell
Get-LocalGroup
```

````

```
Name                                Description
----                                -----------
__vmware__                          VMware User Group
Access Control Assistance Operators Members of this group can remotely query authorization attributes and permission...
Administrators                      Administrators have complete and unrestricted access to the computer/domain
Backup Operators                    Backup Operators can override security restrictions for the sole purpose of back...
Cryptographic Operators             Members are authorized to perform cryptographic operations.
Device Owners                       Members of this group can change system-wide settings.
Distributed COM Users               Members are allowed to launch, activate and use Distributed COM objects on this ...
Event Log Readers                   Members of this group can read event logs from local machine
Guests                              Guests have the same access as members of the Users group by default, except for...
Hyper-V Administrators              Members of this group have complete and unrestricted access to all features of H...
IIS_IUSRS                           Built-in group used by Internet Information Services.
Network Configuration Operators     Members in this group can have some administrative privileges to manage configur...
Performance Log Users               Members of this group may schedule logging of performance counters, enable trace...
Performance Monitor Users           Members of this group can access performance counter data locally and remotely
Power Users                         Power Users are included for backwards compatibility and possess limited adminis...
Remote Desktop Users                Members in this group are granted the right to logon remotely
Remote Management Users             Members of this group can access WMI resources over management protocols (such a...
Replicator                          Supports file replication in a domain
System Managed Accounts Group       Members of this group are managed by the system.
Users                               Users are prevented from making accidental or intentional system-wide changes an...
```

Above is an example of the local groups to a standalone host. We can see there are groups for simple things like Administrators and guest accounts, but also groups for specific roles like administrators for virtualization applications, remote users, etc. Let us interact with users and groups now that we understand them.

## Adding/Removing/Editing User Accounts & Groups

Like most other things in PowerShell, we use the get, new, and set verbs to find, create and modify users and groups. If dealing with local users and groups, localuser & localgroup can accomplish this. For domain assets, aduser & adgroup does the trick. If we were not sure, we could always use the Get-Command _user_ cmdlet to see what we have access to. Let us give a few of them a try.

### Identifying Local Users

```powershell
Get-LocalUser
```

```
Name               Enabled Description
----               ------- -----------
Administrator      False   Built-in account for administering the computer/domain
DefaultAccount     False   A user account managed by the system.
DLarusso           True    High kick specialist.
Guest              False   Built-in account for guest access to the computer/domain
sshd               True
WDAGUtilityAccount False   A user account managed and used by the system for Windows Defender A...
```

Get-LocalUser will display the users on our host. These users only have access to this particular host. Let us say that we want to create a new local user named JLawrence. We can accomplish the task using New-LocalUser. If we are unsure of the proper syntax, please do not forget about the Get-Help Command. When creating a new local user, the only real requirement from a syntax perspective is to enter a name and specify a password (or -NoPassword). All other settings, such as a description or account expiration, are optional.

### Creating A New User

```powershell
New-LocalUser -Name "JLawrence" -NoPassword
```

```
Name      Enabled Description
----      ------- -----------
JLawrence True
```

Above, we created the user JLawrence and did not set a password. So this account is active and can be logged in without a password. Depending on the version of Windows we are using, by not setting a Password, we are flagging to windows that this is a Microsoft live account, and it attempts to login in that manner instead of using a local password.

If we wish to modify a user, we could use the Set-LocalUser cmdlet. For this example, we will modify JLawrence and set a password and description on his account.

### Modifying a User

```powershell
$Password = Read-Host -AsSecureString
****************
Set-LocalUser -Name "JLawrence" -Password $Password -Description "CEO EagleFang"
Get-LocalUser
```

```
Name               Enabled Description
----               ------- -----------
Administrator      False   Built-in account for administering the computer/domain
DefaultAccount     False   A user account managed by the system.
demo               True
Guest              False   Built-in account for guest access to the computer/domain
```
````
