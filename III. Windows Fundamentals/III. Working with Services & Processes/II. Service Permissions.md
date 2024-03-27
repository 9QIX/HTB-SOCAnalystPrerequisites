# Service Permissions

Recall that services allow for the management of long-running processes and are a critical part of Windows operating systems. Sysadmins often overlook them as potential threat vectors that can be used to load malicious DLLs, execute applications without access to an admin account, escalate privileges and even maintain persistence. These threat vectors in Windows services often come into existence through service permissions misconfigurations put in place by 3rd party software and easy to make mistakes by admins during install processes.

The first step in realizing the importance of service permissions is simply understanding that they exist and being mindful of them. On server operating systems, critical network services like DHCP and Active Directory Domain Services commonly get installed using the account assigned to the admin performing the install. Part of the install process includes assigning a specific service to run using the credentials and privileges of a designated user, which by default is set within the currently logged-on user context.

For example, if we are logged on as Bob on a server during DHCP install, then that service will be configured to run as Bob unless specified otherwise. What bad things could come of this? Well, what if Bob leaves the organization or gets fired? The typical business practice would be to disable Bob’s account as part of his exit process. In this case, what would happen to DHCP and other services running using Bob’s account? Those services would fail to start. DHCP or Dynamic Host Configuration Protocol is responsible for leasing IP addresses to computers on the network. If this service stops on a Windows DHCP server, clients requesting an IP address will not receive one. This means a service misconfiguration could lead to downtime and loss of productivity. It is highly recommended to create an individual user account to run critical network services. These are referred to as service accounts.

We should also be mindful of service permissions and the permissions of the directories they execute from because it is possible to replace the path to an executable with a malicious DLL or executable file. Let's examine the permissions of services running on Windows 10 to get an even better understanding of this.

## Examining Services using services.msc

As discussed in the processes and services section, we can use `services.msc` to view and manage just about every detail regarding all services. Let's take a closer look at the service associated with Windows Update (wuauserv).

![alt text](/Images/image-24.png)

### services.msc

Make a note of the different properties available for viewing and configuration. Knowing the service name is especially useful when using command-line tools to examine and manage services. Path to the executable is the full path to the program and command to execute when the service starts. If the NTFS permissions of the destination directory are configured with weak permissions, an attacker could replace the original executable with one created for malicious purposes. We discuss NTFS permissions more in the NTFS vs. Share permissions section of this module.

**Log On tab**

Most services run with LocalSystem privileges by default which is the highest level of access allowed on an individual Windows OS. Not all applications need Local System account-level permissions, so it is beneficial to perform research on a case-by-case basis when considering installing new applications in a Windows environment. It is a good practice to identify applications that can run with the least privileges possible to align with the principle of least privilege.

![alt text](/Images/image-25.png)

Here is one breakdown of the principle of least privilege

Notable built-in service accounts in Windows:

- LocalService
- NetworkService
- LocalSystem

Note: We can also create new accounts and use them for the sole purpose of running a service.

**Recovery tab**

The recovery tab allows steps to be configured should a service fail. Notice how this service can be set to run a program after the first failure. This is yet another vector that an attacker could use to run malicious programs by utilizing a legitimate service.

## Examining services using sc

`Sc` can also be used to configure and manage services. Let's experiment with a few commands.

```plaintext
C:\Users\htb-student>sc qc wuauserv
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: wuauserv
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\WINDOWS\system32\svchost.exe -k netsvcs -p
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Update
        DEPENDENCIES       : rpcss
        SERVICE_START_NAME : LocalSystem
```

```plaintext
C:\Users\htb-student>sc //hostname or ip of box query ServiceName
```

We can also use `sc` to start and stop services.

```plaintext
C:\Users\htb-student> sc stop wuauserv

[SC] OpenService FAILED 5:

Access is denied.
```

Notice how we are denied access from performing this action without running it within an administrative context. If we run a command prompt with elevated privileges, we will be permitted to complete this action.

```plaintext
C:\WINDOWS\system32> sc config wuauserv binPath=C:\Winbows\Perfectlylegitprogram.exe

[SC] ChangeServiceConfig SUCCESS

C:\WINDOWS\system32> sc qc wuauserv

[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: wuauserv
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Winbows\Perfectlylegitprogram.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Update
        DEPENDENCIES       : rpcss
        SERVICE_START_NAME : LocalSystem
```

If we were investigating a situation where we suspected that the system had malware, `sc` would give us the ability to quickly search and analyze commonly targeted services and newly created services. It’s also much more script-friendly than utilizing GUI tools like `services.msc`.

Another helpful way we can examine service permissions using `sc` is through the `sdshow` command.

```plaintext
C:\WINDOWS\system32> sc sdshow wuauserv
```

At an initial glance, the output looks crazy. It almost seems that we have done something wrong in our command, but there is a meaning to this madness. Every named object in Windows is a securable object, and even some unnamed objects are securable. If it's securable in a Windows OS, it will have a security descriptor. Security descriptors identify the object’s owner and a primary group containing a Discretionary Access Control List (DACL) and a System Access Control List (SACL).

Generally, a DACL is used for controlling access to an object, and a SACL is used to account for and log access attempts. This section will examine the DACL, but the same concepts would apply to a SACL.

This amalgamation of characters crunched together and delimited by opened and closed parentheses is in a format known as the Security Descriptor Definition Language (SDDL).

## Examine service permissions using PowerShell

```
PS C:\Users\htb-student> Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List

Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\wuauserv
Owner  : NT AUTHORITY\SYSTEM
Group  : NT AUTHORITY\SYSTEM
Access : BUILTIN\Users Allow  ReadKey
         BUILTIN\Users Allow  -2147483648
         BUILTIN\Administrators Allow  FullControl
         BUILTIN\Administrators Allow  268435456
         NT AUTHORITY\SYSTEM Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  268435456
         CREATOR OWNER Allow  268435456
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadKey
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  -2147483648
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         ReadKey
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         -2147483648
Audit  :
Sddl   : O:SYG:SYD:AI(A;ID;KR;;;BU)(A;CIIOID;GR;;;BU)(A;ID;KA;;;BA)(A;CIIOID;GA;;;BA)(A;ID;KA;;;SY)(A;CIIOID;GA;;;SY)(A
         ;CIIOID;GA;;;CO)(A;ID;KR;;;AC)(A;CIIOID;GR;;;AC)(A;ID;KR;;;S-1-15-3-1024-1065365936-1281604716-3511738428-1654
         721687-432734479-3232135806-4053264122-3456934681)(A;CIIOID;GR;;;S-1-15-3-1024-1065365936-1281604716-351173842
         8-1654721687-432734479-3232135806-4053264122-3456934681)
```

Knowing how to interact with services and their associated permissions from the command line makes it easier to script out these tasks. While it is good to know how to perform these tasks from the GUI, it does not scale well as we start getting into larger network environments and Domains.
