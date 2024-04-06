# Networking Management from The CLI

PowerShell has expanded our capabilities within the Windows OS when dealing with Networking settings, applications, and more. This section will cover how to check your network settings, such as IP addresses, adapter settings, and DNS settings. We will also cover How to enable and manage remote host access utilizing WinRM and SSH.

Scenario: To ensure Mr. Tanaka's host is functioning properly and we can manage it from the IT office remotely, we are going to perform a quick checkup, validate his host settings, and enable remote management for the host.

## What Is Networking Within a Windows Network?

Networking with Windows hosts functions much like any other Linux or Unix-based host. The TCP/IP stack, wireless protocols, and other applications treat most devices the same, so there isn't much to learn there that's new. This module assumes you know basic networking protocols and how typical network traffic traverses the Internet. If you wish for a primer on networking, check out the Introduction to Networking module, or for a more in-depth dissection of network traffic, you can play through the Introduction to Network Traffic Analysis module. Where things get a bit different lies in how Windows hosts communicate with each other, domains, and other Linux hosts. Below we will quickly cover some standard protocols you could run into when administering or pentesting Windows hosts.

| Protocol   | Description                                                                                                                                                                                                                                                                                                                       |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SMB        | SMB provides Windows hosts with the capability to share resources, files, and a standard way of authenticating between hosts to determine if access to resources is allowed. For other distros, SAMBA is the open-source option.                                                                                                  |
| Netbios    | NetBios itself isn't directly a service or protocol but a connection and conversation mechanism widely used in networks. It was the original transport mechanism for SMB, but that has since changed. Now it serves as an alternate identification mechanism when DNS fails. Can also be known as NBT-NS (NetBIOS name service).  |
| LDAP       | LDAP is an open-source cross-platform protocol used for authentication and authorization with various directory services. This is how many different devices in modern networks can communicate with large directory structure services such as Active Directory.                                                                 |
| LLMNR      | LLMNR provides a name resolution service based on DNS and works if DNS is not available or functioning. This protocol is a multicast protocol and, as such, works only on local links ( within a normal broadcast domain, not across layer three links).                                                                          |
| DNS        | DNS is a common naming standard used across the Internet and in most modern network types. DNS allows us to reference hosts by a unique name instead of their IP address. This is how we can reference a website by "WWW.google.com" instead of "8.8.8.8". Internally this is how we request resources and access from a network. |
| HTTP/HTTPS | HTTP/S HTTP and HTTPS are the insecure and secure way we request and utilize resources over the Internet. These protocols are used to access and utilize resources such as web servers, send and receive data from remote sources, and much more.                                                                                 |
| Kerberos   | Kerberos is a network level authentication protocol. In modern times, we are most likely to see it when dealing with Active Directory authentication when clients request tickets for authorization to use domain resources.                                                                                                      |
| WinRM      | WinRM Is an implementation of the WS-Management protocol. It can be used to manage the hardware and software functionalities of hosts. It is mainly used in IT administration but can also be used for host enumeration and as a scripting engine.                                                                                |
| RDP        | RDP is a Windows implementation of a network UI services protocol that provides users with a Graphical interface to access hosts over a network connection. This allows for full UI use to include the passing of keyboard and mouse input to the remote host.                                                                    |
| SSH        | SSH is a secure protocol that can be used for secure host access, transfer of files, and general communication between network hosts. It provides a way to securely access hosts and services over insecure networks.                                                                                                             |

Of course, this list is not all-encompassing, but it is an excellent general start of what we would typically see when communicating with Windows hosts. Now let's discuss local access vs. remote access.

## Local vs. Remote Access?

### Local Access

Local host access is when we are directly at the terminal utilizing its resources as you are right now from your PC. Usually, this will not require us to use any specific access protocols except when we request resources from networked hosts or attempt to access the Internet. Below we will showcase some cmdlets and other ways to check and validate network settings on our hosts.

### Querying Networking Settings

Before doing anything else, let's validate the network settings on Mr. Tanaka's host. We will start by running the IPConfig command. This isn't a PowerShell native command, but it is compatible.

#### IPConfig

```powershell
PS C:\htb> ipconfig

Windows IP Configuration

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : .htb
   Link-local IPv6 Address . . . . . : fe80::c5ca:594d:759d:e0c1%11
   IPv4 Address. . . . . . . . . . . : 10.129.203.105
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:b9fc%11
                                       10.129.0.1
```

As we can see, ipconfig will show us the basic settings of your network interface. We have as output the IPv4/6 addresses, our gateway, subnet masks, and DNS suffix if one is set. We can output the full network settings by appending the `/all` modifier to the ipconfig command like so:

```powershell
PS C:\htb> ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : ICL-WIN11
   Primary Dns Suffix  . . . . . . . : greenhorn.corp
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : greenhorn.corp
                                       htb

Ethernet adapter Ethernet0:

   Connection-specific DNS Suffix  . : .htb
   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter
   Physical Address. . . . . . . . . : 00-50-56-B9-4F-CB
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : dead:beef::222(Preferred)
   Lease Obtained. . . . . . . . . . : Monday, October 17, 2022 9:40:14 AM
   Lease Expires . . . . . . . . . . : Tuesday, October 25, 2022 9:59:17 AM
   <SNIP>
   IPv4 Address. . . . . . . . . . . : 10.129.203.105(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Lease Obtained. . . . . . . . . . : Monday, October 17, 2022 9:40:13 AM
   Lease Expires . . . . . . . . . . : Tuesday, October 25, 2022 10:10:16 AM
   Default Gateway . . . . . . . . . : fe80::250:56ff:feb9:b9fc%11
                                       10.129.0.1
   DHCP Server . . . . . . . . . . . : 10.129.0.1
   DHCPv6 IAID . . . . . . . . . . . : 335564886
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2A-3D-00-D6-00-50-56-B9-4F-CB
   DNS Servers . . . . . . . . . . . : 1.1.1.1
                                       8.8.8.8
   NetBIOS over Tcpip. . . . . . . . : Enabled
   Connection-specific DNS Suffix Search List :
                                       htb

Ethernet adapter Ethernet2:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : vmxnet3 Ethernet Adapter #2
   Physical Address. . . . . . . . . : 00-50-56-B9-F5-7E
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::d1fb:79d5:6d0b:41de%14(Preferred)
   IPv4 Address. . . . . . . . . . . : 172.16.5.100(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 172.16.5.1
   DHCPv6 IAID . . . . . . . . . . . : 318787670
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-2A-3D-00-D6-00-50-56-B9-4F-CB
   DNS Servers . . . . . . . . . . . : 172.16.5.155
   NetBIOS over Tcpip. . . . . . . . : Enabled
```

Now we can see much more information than before. We are presented with output containing multiple adapters, Host settings, more details about if our IP addresses were manually set or DHCP leases, how long those leases are, and more. So, it appears Mr. Tanaka's host has a proper IP address configuration. Of note, and particularly interesting to us as pentesters, is that this host is dual-homed. We mean it has multiple network interfaces connected to separate networks. This makes Mr. Tanakas host a great target if we are looking for a foothold in the network and wish to have a way to migrate between networks.

Let's look at Arp settings and see if his host has communicated with others on the network. As a refresher, ARP is a protocol utilized to translate IP addresses to Physical addresses. The physical address is used at lower levels of the OSI/TCP-IP models for communication. To have it display the host's current ARP entries, we will use the `-a` switch.

#### ARP

```powershell
PS C:\htb> arp -a

Interface: 10.129.203.105 --- 0xb
  Internet Address      Physical Address      Type
  10.129.0.1            00-50-56-b9-b9-fc     dynamic
  10.129.204.58         00-50-56-b9-5f-41     dynamic
  10.129.255.255        ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
  255.255.255.255       ff-ff-ff-ff-ff-ff     static

Interface: 172.16.5.100 --- 0xe
  Internet Address      Physical Address      Type
  172.16.5.155          00-50-56-b9-e2-30     dynamic
  172.16.5.255          ff-ff-ff-ff-ff-ff     static
  224.0.0.22            01-00-5e-00-00-16     static
  224.0.0.251           01-00-5e-00-00-fb     static
  224.0.0.252           01-00-5e-00-00-fc     static
  239.255.255.250       01-00-5e-7f-ff-fa     static
```

The output from Arp `-a` is pretty simple. We are provided with entries from our network adapters about the hosts it is aware of or has communicated with recently. Not surprisingly, since this host is fairly new, it has yet to communicate with too many hosts. Just the gateways, our remote host, and the host 172.16.5.155, the Domain Controller for Greenhorn.corp. Nothing crazy to be seen here. Now let's validate our DNS configuration is working properly. We will utilize nslookup, a built-in DNS querying tool, to attempt to resolve the IP address / DNS name of the Greenhorn domain controller.

#### Nslookup

```powershell
PS C:\htb> nslookup ACADEMY-ICL-DC

DNS request timed out.
    timeout was 2 seconds.
Server:  UnKnown
Address:  172.16.5.155

Name:    ACADEMY-ICL-DC.greenhorn.corp
Address:  172.16.5.155
```

Now that we have validated Mr. Tanakas DNS settings, let's check the open ports on the host. We can do so using `netstat -an`. Netstat will display current network connections to our host. The `-an` switch will print all connections and listening ports and place them in numerical form.

#### Netstat

```powershell
PS C:\htb> netstat -an

netstat -an

Active Connections

  Proto  Local Address          Foreign Address        State
  TCP    0.0.0.0:22             0.0.0.0:0              LISTENING
  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING
  TCP    0.0.0.0:3389           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:5040           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:5985           0.0.0.0:0              LISTENING
  TCP    0.0.0.0:47001          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49664          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49665          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49666          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49667          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49668          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49671          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49673          0.0.0.0:0              LISTENING
  TCP    0.0.0.0:49674          0.0.0.0:0              LISTENING
  TCP    10.129.203.105:22      10.10.14.19:32557      ESTABLISHED
  TCP    172.16.5.100:139       0.0.0.0:0              LISTENING
  TCP    [::]:22                [::]:0                 LISTENING
  TCP    [::]:135               [::]:0                 LISTENING
  TCP    [::]:445               [::]:0                 LISTENING
  TCP    [::]:3389              [::]:0                 LISTENING
  TCP    [::]:5985              [::]:0                 LISTENING
  TCP    [::]:47001             [::]:0                 LISTENING
  TCP    [::]:49664             [::]:0                 LISTENING
  TCP    [::]:49665             [::]:0                 LISTENING
  TCP    [::]:49666             [::]:0                 LISTENING
  TCP    [::]:49667             [::]:0                 LISTENING
  TCP    [::]:49668             [::]:0                 LISTENING
  TCP    [::]:49671             [::]:0                 LISTENING
  TCP    [::]:49673             [::]:0                 LISTENING
  TCP    [::]:49674             [::]:0                 LISTENING
  UDP    0.0.0.0:123            *:*
<SNIP>
  UDP    172.16.5.100:137       *:*
  UDP    172.16.5.100:
```
