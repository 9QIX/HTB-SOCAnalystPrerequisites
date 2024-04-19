# User and Machine Accounts

User accounts are created on both local systems (not joined to AD) and in Active Directory to give a person or a program (such as a system service) the ability to log on to a computer and access resources based on their rights. When a user logs in, the system verifies their password and creates an access token. This token describes the security content of a process or thread and includes the user's security identity and group membership. Whenever a user interacts with a process, this token is presented. User accounts are used to allow employees/contractors to log in to a computer and access resources, to run programs or services under a specific security context (i.e., running as a highly privileged user instead of a network service account), and to manage access to objects and their properties such as network file shares, files, applications, etc. Users can be assigned to groups that can contain one or more members. These groups can also be used to control access to resources. It can be easier for an administrator to assign privileges once to a group (which all group members inherit) instead of many times to each individual user. This helps simplify administration and makes it easier to grant and revoke user rights.

The ability to provision and manage user accounts is one of the core elements of Active Directory. Typically, every company we encounter will have at least one AD user account provisioned per user. Some users may have two or more accounts provisioned based on their job role (i.e., an IT admin or Help Desk member). Aside from standard user and admin accounts tied back to a specific user, we will often see many service accounts used to run a particular application or service in the background or perform other vital functions within the domain environment. An organization with 1,000 employees could have 1,200 active user accounts or more! We may also see organizations with hundreds of disabled accounts from former employees, temporary/seasonal employees, interns, etc. Some companies must retain records of these accounts for audit purposes, so they will deactivate them (and hopefully remove all privileges) once the employee is terminated, but they will not delete them. It is common to see an OU such as FORMER EMPLOYEES that will contain many deactivated accounts.

![alt text](/Images/image-78.png)

As we will see later in this module, user accounts can be provisioned many rights in Active Directory. They can be configured as basically read-only users who have read access to most of the environment (which are the permissions a standard Domain User receives) up to Enterprise Admin (with complete control of every object in the domain) and countless combinations in between. Because users can have so many rights assigned to them, they can also be misconfigured relatively easily and granted unintended rights that an attacker or a penetration tester can leverage. User accounts present an immense attack surface and are usually a key focus for gaining a foothold during a penetration test. Users are often the weakest link in any organization. It is difficult to manage human behavior and account for every user choosing weak or shared passwords, installing unauthorized software, or admins making careless mistakes or being overly permissive with account management. To combat this, an organization needs to have policies and procedures to combat issues that can arise around user accounts and must have defense in depth to mitigate the inherent risk that users bring to the domain.

Specifics on user-related misconfigurations and attacks are outside the scope of this module. Still, it is important to understand the sheer impact users can have within any Active Directory network and to understand the nuances between the different types of users/accounts we may encounter.

## Local Accounts

Local accounts are stored locally on a particular server or workstation. These accounts can be assigned rights on that host either individually or via group membership. Any rights assigned can only be granted to that specific host and will not work across the domain. Local user accounts are considered security principals but can only manage access to and secure resources on a standalone host. There are several default local user accounts that are created on a Windows system:

- **Administrator**: this account has the SID S-1-5-domain-500 and is the first account created with a new Windows installation. It has full control over almost every resource on the system. It cannot be deleted or locked, but it can be disabled or renamed. Windows 10 and Server 2016 hosts disable the built-in administrator account by default and create another local account in the local administrator's group during setup.

- **Guest**: this account is disabled by default. The purpose of this account is to allow users without an account on the computer to log in temporarily with limited access rights. By default, it has a blank password and is generally recommended to be left disabled because of the security risk of allowing anonymous access to a host.

- **SYSTEM**: The SYSTEM (or NT AUTHORITY\SYSTEM) account on a Windows host is the default account installed and used by the operating system to perform many of its internal functions. Unlike the Root account on Linux, SYSTEM is a service account and does not run entirely in the same context as a regular user. Many of the processes and services running on a host are run under the SYSTEM context. One thing to note with this account is that a profile for it does not exist, but it will have permissions over almost everything on the host. It does not appear in User Manager and cannot be added to any groups. A SYSTEM account is the highest permission level one can achieve on a Windows host and, by default, is granted Full Control permissions to all files on a Windows system.

- **Network Service**: This is a predefined local account used by the Service Control Manager (SCM) for running Windows services. When a service runs in the context of this particular account, it will present credentials to remote services.

- **Local Service**: This is another predefined local account used by the Service Control Manager (SCM) for running Windows services. It is configured with minimal privileges on the computer and presents anonymous credentials to the network.

It is worth studying Microsoft's documentation on local default accounts in-depth to gain a better understanding of how the various accounts work together on an individual Windows system and across a domain network. Take some time to look them over and understand the nuances between them.

## Domain Users

Domain users differ from local users in that they are granted rights from the domain to access resources such as file servers, printers, intranet hosts, and other objects based on the permissions granted to their user account or the group that account is a member of. Domain user accounts can log in to any host in the domain, unlike local users. For more information on the many different Active Directory account types, check out [this link](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-accounts). One account to keep in mind is the KRBTGT account, however. This is a type of local account built into the AD infrastructure. This account acts as a service account for the Key Distribution service providing authentication and access for domain resources. This account is a common target of many attackers since gaining control or access will enable an attacker to have unconstrained access to the domain. It can be leveraged for privilege escalation and persistence in a domain through attacks such as the Golden Ticket attack.

## User Naming Attributes

Security in Active Directory can be improved using a set of user naming attributes to help identify user objects like logon name or ID. The following are a few important Naming Attributes in AD:

- **UserPrincipalName (UPN)**: This is the primary logon name for the user. By convention, the UPN uses the email address of the user.
- **ObjectGUID**: This is a unique identifier of the user. In AD, the ObjectGUID attribute name never changes and remains unique even if the user is removed.
- **SAMAccountName**: This is a logon name that supports the previous version of Windows clients and servers.
- **objectSID**: The user's Security Identifier (SID). This attribute identifies a user and its group memberships during security interactions with the server.
- **sIDHistory**: This contains previous SIDs for the user object if moved from another domain and is typically seen in migration scenarios from domain to domain. After a migration occurs, the last SID will be added to the sIDHistory property, and the new SID will become its objectSID.

## Common User Attributes

```powershell
PS C:\htb> Get-ADUser -Identity htb-student

DistinguishedName : CN=htb student,CN=Users,DC=INLANEFREIGHT,DC=LOCAL
Enabled           : True
GivenName         : htb
Name              : htb student
ObjectClass       : user
ObjectGUID        : aa799587-c641-4c23-a2f7-75850b4dd7e3
SamAccountName    : htb-student
SID               : S-1-5-21-3842939050-3880317879-2865463114-1111
Surname           : student
UserPrincipalName : htb-student@INLANEFREIGHT.LOCAL
```

For a deeper look at user object attributes, check out [this page](https://docs.microsoft.com/en-us/windows/desktop/ADSchema/user-object). Many attributes can be set for any object in AD. Many objects will never be used or are not relevant to us as security professionals. Still, it is essential to familiarize ourselves with the most common and more obscure ones that may contain sensitive data or help mount an attack.

## Domain-joined vs. Non-Domain-joined Machines

When it comes to computer resources, there are several ways they are typically managed. Below we will discuss the differences between a host joined to a domain versus a host that is only in a workgroup.

### Domain joined

Hosts joined to a domain have greater ease of information sharing within the enterprise and a central management point (the DC) to gather resources, policies, and updates from. A host joined to a domain will acquire any configurations or changes necessary through the domain's Group Policy. The benefit here is that a user in the domain can log in and access resources from any host joined to the domain, not just the one they work on. This is the typical setup you will see in enterprise environments.

### Non-domain joined

Non-domain joined computers or computers in a workgroup are not managed by domain policy. With that in mind, sharing resources outside your local network is much more complicated than it would be on a domain. This is fine for computers meant for home use or small business clusters on the same LAN. The advantage of this setup is that the individual users are in charge of any changes they wish to make to their host. Any user accounts on a workgroup computer only exist on that host, and profiles are not migrated to other hosts within the workgroup.

It is important to note that a machine account (NT AUTHORITY\SYSTEM level access) in an AD environment will have most of the same rights as a standard domain user account. This is important because we do not always need to obtain a set of valid credentials for an individual user's account to begin enumerating and attacking a domain (as we will see in later modules). We may obtain SYSTEM level access to a domain-joined Windows host through a successful remote code execution exploit or by escalating privileges on a host. This access is often overlooked as only useful for pillaging sensitive data (i.e., passwords, SSH keys, sensitive files, etc.) on a particular host. In reality, access in the context of the SYSTEM account will allow us read access to much of the data within the domain and is a great launching point for gathering as much information about the domain as possible before proceeding with applicable AD-related attacks.
