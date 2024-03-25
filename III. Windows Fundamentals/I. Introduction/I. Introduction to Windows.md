# Introduction to Windows

As a penetration tester, understanding Windows operating systems is crucial for various assessment types. Windows, alongside Linux, constitutes a significant portion of systems encountered during assessments, whether on-premise or in the cloud. Having a grasp of how to both attack and defend Windows systems is essential for effective penetration testing activities.

#### The Windows Operating System

Microsoft introduced the Windows operating system on November 20, 1985. Initially serving as a graphical shell for MS-DOS, Windows evolved over the years. Windows 95 marked the integration of Windows and DOS, offering built-in Internet support and debuting Internet Explorer. Subsequent versions, including Windows XP, Vista, 8, and the current Windows 10, introduced various features catering to different user segments, from casual consumers to enterprise customers.

Windows Server, starting with Windows NT 3.1 Advanced Server in 1993, underwent significant updates, introducing technologies like Internet Information Services (IIS) and Active Directory. With each iteration, new features and enhancements, such as failover clustering, Hyper-V virtualization, and advanced security features, were introduced, culminating in the latest release, Windows Server 2019.

#### Windows Versions

Windows versions are identified by version numbers associated with their respective names. Here's a list of major Windows operating systems and their version numbers:

Here's the information presented in a Markdown table:

| Operating System Name                | Version Number |
| ------------------------------------ | -------------- |
| Windows NT 4                         | 4.0            |
| Windows 2000                         | 5.0            |
| Windows XP                           | 5.1            |
| Windows Server 2003, 2003 R2         | 5.2            |
| Windows Vista, Server 2008           | 6.0            |
| Windows 7, Server 2008 R2            | 6.1            |
| Windows 8, Server 2012               | 6.2            |
| Windows 8.1, Server 2012 R2          | 6.3            |
| Windows 10, Server 2016, Server 2019 | 10.0           |

This table provides a summary of various Windows operating systems along with their version numbers.

We can use the Get-WmiObject cmdlet to find information about the operating system. This cmdlet can be used to get instances of WMI classes or information about available WMI classes. There are a variety of ways to find the version and build number of our system. We can easily obtain this information using the win32_OperatingSystem class, which shows that we are on a Windows 10 host, build number 19041.

```ps1
PS C:\htb> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

Version BuildNumber

---

10.0.19041 19041
```

Some other useful classes that can be used with Get-WmiObject are Win32_Process to get a process listing, Win32_Service to get a listing of services, and Win32_Bios to get Basic Input/Output System (BIOS) information. The BIOS is firmware installed on a computer's motherboard that controls the computer's essential functions, such as power management, input/output interfaces, and system configuration. We can use the ComputerName parameter to get information about remote computers. Get-WmiObject can be used to start and stop services on local and remote computers, and more. Further information about the cmdlet can be found here and here.

#### Accessing Windows

Accessing Windows systems involves local or remote access. Local access is the most common, where input and output occur through physical devices like keyboards, mice, and displays. Remote access, on the other hand, involves accessing a computer over a network. Common remote access methods include:

- Virtual Private Networks (VPN)
- Secure Shell (SSH)
- File Transfer Protocol (FTP)
- Virtual Network Computing (VNC)
- Windows Remote Management (or PowerShell Remoting) (WinRM)
- Remote Desktop Protocol (RDP)
- We will focus primarily on using RDP in this module.

#### Remote Desktop Protocol (RDP)

RDP enables remote access to Windows systems over a network. It utilizes a client/server architecture, with the target computer being the server. RDP listens on port 3389 by default. Remote Desktop Connection, the built-in RDP client in Windows, allows users to connect to remote systems conveniently. Tools like xfreerdp facilitate RDP connections from Linux-based hosts, providing efficiency and flexibility.

Understanding Windows operating systems, accessing them locally or remotely, and leveraging tools like RDP are essential skills for penetration testers to effectively assess Windows environments.

We can use RDP to connect to a Windows target from an attack host running Linux or Windows. If we are connecting to a Windows target from a Windows host, we can use the built-in RDP client application called Remote Desktop Connection (mstsc.exe).

As pentesters, we can benefit from looking for these saved Remote Desktop Files (.rdp) while on an engagement.

Many other Remote Desktop client applications exist, some of which are listed in this Microsoft article called Remote Desktop clients. We will not cover every Remote Desktop client application in this module.

#### Using xfreerdp

From a Linux-based attack host we can use a tool called xfreerdp to remotely access Windows targets. You will notice that we use xfreerdp across multiple modules because of its ease of use, feature set, command line utility, and efficiency.

Remember that we can also copy and paste in xfreerdp commands in the command line, so we do not need to enter options manually. There are several options available to us with xfreerdp, such as drive redirection to be able to tranfer files to/from the target host, which are worth practicing and we will cover in other modules within HTB Academy.

Other RDP clients exist, such as Remmina and rdesktop, and we encourage you to experiment with others and see what works best for you. Now that we have covered these concepts let's apply them by spawning the target below and connecting to it using RDP with the credentials provided.

#### Connecting to the Windows Target

Connect via Remote Desktop (RDP) using the following command:

```bash
[!bash!]$ xfreerdp /v:<targetIp> /u:htb-student /p:Password
```

Note: It may take 1-2 minutes for your target instance to spawn.

#### Activity

- What is the Build Number of the target workstation?

```ps1
Get-WmiObject -Class win32_OperatingSystem | selectBuildNumber
```

- Which Windows NT version is installed on the workstation? (i.e. Windows X - case sensitive)
