# Vendor Specific Information

Cisco IOS is the operating system of Cisco network devices such as routers and switches. It provides features and services required to manage and operate network devices. This operating system comes in different versions and releases that vary in features, support, and performance. It offers several features required for the operation of modern networks, such as, but not limited to:

- Support for IPv6
- Quality of Service (QoS)
- Security features such as encryption and authentication
- Virtualization features such as Virtual Private LAN Service (VPLS)
- Virtual Routing and Forwarding (VRF)

Cisco IOS can be managed in several ways, depending on the network device and hardware used. The most commonly used method is the command line interface (CLI), which can also be managed in the graphical user interface (GUI). In addition, it supports various network protocols and services required for network operations. These include:

| Protocol Type       | Description                                                                                                                                                     |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Routing protocols   | Such as OSPF and BGP are used to route data packets on a network.                                                                                               |
| Switching protocols | Such as VLAN Trunking Protocol (VTP) and Spanning Tree Protocol (STP) is used to configure and manage switches on a network.                                    |
| Network services    | Such as Dynamic Host Configuration Protocol (DHCP) are used to automatically provide clients on the network with IP addresses and other network configurations. |
| Security features   | Such as Access Control Lists (ACLs), which are used to control access to network resources and prevent security threats.                                        |

In Cisco IOS, different types of passwords are used for various purposes, for example:

| Password Type   | Description                                                                                                                                               |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| User            | The user password is used for logging in to Cisco IOS. It is used to restrict access to the network device and its features.                              |
| Enable Password | The enable password is used to enter "enable" mode. The "enable" mode is the mode where you have access to advanced functions and settings.               |
| Secret          | The secret is a password to secure access to certain functions and services. It is often used to restrict access to remote management tools and services. |
| Enable Secret   | The enable secret is an extra-secure password used to secure access to "enable" mode, and they are stored encrypted to provide additional protection.     |

We highly recommend going through the provided external resources to understand the encryption mechanics of Cisco IOS and how those are used.

The Cisco IOS devices can be configured for SSH or Telnet. So it can be accessed remotely. We can determine from the response we receive that it is indeed a Cisco IOS, as it responds with the User Access Verification message.

```bash
$ telnet 10.129.10.2

Trying 10.129.10.2...
Connected to 10.129.10.2.
Escape character is '^]'.


User Access Verification

Password:
```

VLANs
Imagine this scenario: A startup called XQ hired a network administrator to create a network for their single-office company, and due to budget limitations, they can only afford one switch and router. The sysadmin of XQ stated that in addition to hosting the web and database servers in the network, staff from different departments will be using it. As a seasoned network security specialist, the network administrator immediately thought about the security attacks that an insider can perform, especially ones abusing broadcast traffic, such as broadcast storms. Therefore, to tackle this problem, the network administrator decided to logically segment the network with Virtual Local Area Networks (VLANS), conceptually breaking down one switch into smaller mini-switches.

A VLAN is a logical grouping of network endpoints connected to defined ports on a switch, allowing the segmentation of networks by creating logical broadcast domains that can span multiple physical LAN segments. With VLANs, network administrators can segment networks based on factors such as team, function, department, or application, without worrying about the physical location of endpoints and users. A broadcast packet sent over one VLAN does not reach any other endpoint that is a member of another VLAN. Because each VLAN is regarded as a broadcast domain, it needs to have its own subnet; for example, the network administrator contracted by XQ can segment the network by departments:

| Department | VLAN ID | Subnet         |
| ---------- | ------- | -------------- |
| Servers    | VLAN 10 | 192.168.1.0/24 |
| C-Level    | VLAN 20 | 192.168.2.0/24 |
| Finance    | VLAN 30 | 192.168.3.0/24 |
| HR         | VLAN 40 | 192.168.4.0/24 |
| Marketing  | VLAN 50 | 192.168.5.0/24 |
| Support    | VLAN 60 | 192.168.6.0/24 |

A myriad of benefits is attained when using VLANs, including:

- Better Organization: Network administrators can group endpoints based on any common attribute they share.
- Increased Security: Network segmentation disallows unauthorized members from sniffing network packets in other VLANs.
- Simplified Administration: Network administrators do not have to worry about the physical locations of an endpoint.
- Increased Performance: With reduced broadcast traffic for all endpoints, more bandwidth is made available for use by the network.

Cisco switches provide the VLAN IDs/numbers 1-4094 (0 and 4095 are reserved IDs and cannot be used); IDs 1-1005 (VLAN 1 is known as the default VLAN and cannot/should not be altered nor deleted) are known as normal-range VLANs, with IDs 1002-1005 being reserved for Token Ring and Fiber Distributed Data Interface (FDDI) VLANs, while IDs 1006-4094 are known as extended-range VLANs. By default, any customization applied for normal-range VLANs is saved in the VLAN database (the vland.dat file), in contrast to extended-range VLANs, which do not have their customizations saved. VLANs 2-1001 stored in vlan.dat can have parameters including name, type, state, and maximum transmission unit (MTU).

VLAN Memberships
Network administrators can assign the ports of a switch to VLANs either statically or dynamically. Static VLAN assignment, which is the simplest and most common method, involves assigning each port to a VLAN manually using the switch's network operating system; this must be done for all switches separately (it is essential to keep in mind that endpoints connecting to these ports are unaware of the existence of VLANs). In contrast, dynamic VLAN assignment automatically determines an endpoint's VLAN membership based on MAC addresses or protocols. The system administrator can register the MAC addresses in a centralized VLAN management service/database, such as the VLAN Membership Policy Server (VMPS) service, and then the switch queries the database of VMPS to determine the VLAN of the endpoint with that specific MAC address. Regardless of their flexibility and mobility, dynamic VLANs increase administrative overhead.

Security-wise, static VLANs are the more secure option because a port will forever be tied to a specific VLAN ID, unless changed manually afterward. For dynamic VLANs, an attacker could potentially utilize tools such as macchanger to spoof the MAC address of legitimate endpoints and attain membership of their VLANs, therefore sniffing all network traffic sent through them.

Access and Trunk Ports
Any port on a VLAN-enabled switch must be either an access port or a trunk port. Access ports belong to and can carry the traffic of only one VLAN (or in some cases two, with the second being for voice traffic); any traffic arriving on an access port is assumed to belong to the VLAN the port was assigned. On the other hand, trunk ports can carry multiple VLANs at the same time; trunk links connect two trunk ports on two switches (or a switch and router) to allow information from multiple VLANs to be carried out across switches.

VLAN Identification
Standard 802.3 Ethernet frames do not contain VLAN information; therefore, switches and other VLAN-enabled devices need a mechanism to keep track of all the VLAN information associated with a packet while traversing VLAN-enabled devices. Two main trunking methods are utilized to achieve this, ISL and IEEE 802.1Q.

### Inter-Switch Link (ISL)

Inter-Switch Link (ISL) is a Cisco-proprietary protocol used for trunking between VLAN-enabled devices. Although ISL is one of the first trunking methods (predating 802.1Q), it is deprecated and not as widely used in modern Cisco switches (and routers). Instead, most only support the widely adopted 802.1Q. ISL encapsulated the entire Ethernet frame, including the original Ethernet header and the VLAN tag, adding its 26-byte header and 4-byte trailer.

### IEEE 802.1Q

To ensure interoperability of VLAN technologies from the various network-equipments vendors, the Institute of Electrical and Electronics Engineers (IEEE) developed the 802.1Q specification in 1998. The IEEE 802 committee had to change the 802.3 Ethernet frame format by adding a pair of 2-byte fields, TPID and TCI (which consists of three subfields, PCP, DEI, and VID), resulting in a VLAN-compliant 802.1Q Ethernet frame.

![8023_Legacy_8021Q_Ethernet_Frames](8023_Legacy_8021Q_Ethernet_Frames.png)

Tag protocol identifier (TPID) is a 16-bit field always set to 0x8100 to identify the Ethernet frame as an 802.1Q-tagged frame. Tag Control Information (TCI) is a 16-bit field containing Priority code point (PCP), Drop eligible indicator (DEI) (previously known as Canonical format indicator (CFI)), and VLAN identifier (VID). The main field concerning VLANs is VID, occupying the low-order 12-bits of TCI. Since it is 12 bits, it allows 2^12 - 2 = 4096 (remember, 0 and 4095 are reserved) VLAN IDs. Therefore, an 802.1Q-tagged frame can contain information for 4094 VLANs; the practice of inserting multiple 802.1Q tags within a single packet is known as Double Tagging, introduced by 802.1ad. VLAN tagging is the process of inserting VLAN information into an 802.1Q Ethernet header, while VLAN untagging is the process of removing the VLAN information from an 802.1Q-tagged Ethernet frame and forwarding the packet to the destined ports.

VLAN-Capable NICs
Some network interface cards (NICs) attached to computers/servers support VLAN tagging. Let us see how we can assign a VLAN ID to a NIC using Linux and Windows.

### Assigning NICs a VLAN in Linux

In Linux, creating a VLAN is done by creating an interface on top of another, called a parent interface. This VLAN interface will tag packets with the assigned VLAN ID while returning packets will be untagged.

To assign a network adapter a VLAN in Linux, many tools can be used, such as ip, nmcli, and vconfig (deprecated). However, first, we need to ensure that the Kernel has the 802.1Q module loaded:

```bash
$ sudo modprobe 8021q
```

Subsequently, we can use `lsmod` to make sure `8021q` was loaded successfully:

```bash
$ lsmod | grep 8021

8021q                  40960  0
garp                   16384  1 8021q
mrp                    20480  1 8021q
```

Now, we need to find the name of the physical Ethernet interface that we will create the VLAN interface on top of, which is `eth0`:

```bash
$ ip a

<SNIP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether a6:ba:3b:08:3a:36 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    altname ens3
    inet 94.2X.5X.72/22 brd 94.237.51.255 scope global dynamic eth0
       valid_lft 83489sec preferred_lft 83489sec
    inet6 fe80::a4ba:3bff:fe08:3a36/64 scope link
       valid_lft forever preferred_lft forever
```

Then, we will use `vconfig` to create a new interface that is a member of the desired VLAN, 20, for example, on top of `eth0`:

```bash
$ sudo vconfig add eth0 20

Warning: vconfig is deprecated and might be removed in the future, please migrate to ip(route2) as soon as possible!
```

To use `ip` instead:

```bash
sudo ip link add link eth0 name eth0.20 type vlan id 20
```

Either of these commands will make a new interface called `eth0.20@eth0`:

```bash
$ ip a

<SNIP>
4: eth0.20@eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether a6:ba:3b:08:3a:36 brd ff:ff:ff:ff:ff:ff
```

Then, based on the subnet assigned to the addresses with VLAN 20 within the local network, we need to assign the interface an IP address and then start it:

```bash
$ sudo ip addr add 192.168.1.1/24 dev eth0.20
$ sudo ip link set up eth0.20
```

At last, we can check whether the interface has changed states to up:

```bash
$ ip a | grep eth0.20

4: eth0.20@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    inet 192.168.1.1/24 scope global eth0.20
```

### Assigning NICs a VLAN in Windows

On Windows, to assign a VLAN for a physical network adapter that supports VLAN tagging, first we need to open Device Manager:

![Windows_Device_Manager](Windows_Device_Manager.png)

Then we need to click on Properties for the Ethernet interface we want to assign to a VLAN:

![Windows_Device_Manager_Adapter_Properties](Windows_Device_Manager_Adapter_Properties.png)

Within Advanced, there will be a VLAN ID property to which we can assign a value. After clicking OK, if the adapter supports assigning a VLAN, it will be set; otherwise, the window will close, and no VLAN tag will be added to any packets originating from this host:

![Windows_Device_Manager_Adapter_Properties_VLAN_ID](Windows_Device_Manager_Adapter_Properties_VLAN_ID.png)

Instead of relying on the GUI, we can use PowerShell. First, let us get the names of all the available physical network adapters using the `Get-NetAdapter` Cmdlet:

```powershell
PS C:\> Get-NetAdapter | Format-Table -AutoSize

Name                                           InterfaceDescription                                                          ifIndex Status             MacAddress              LinkSpeed
----                                           --------------------                                                          ------- ------             ----------              ---------
VirtualBox Host-Only Network  VirtualBox Host-Only Ethernet Adapter                                        20 Up                    0A-00-27-10-42-15       1 Gbps
Ethernet 2                                 ASIX AX88772B USB2.0 to Fast Ethernet Adapter                            55 Up                    90-EB-78-14-21-7F    100 Mbps
Bluetooth Network Connection  Bluetooth Device (Personal Area Network)                                   18 Disconnected   38-41-25-E8-DE-2D        3 Mbps
Wi-Fi                                         Intel(R) Wireless-AC 9560 160MHz                                                12 Disconnected   8E-36-6A-7A-BA-6A 866.7 Mbps
```

Previously, we used Device Manager to assign Ethernet 2 to VLAN 10; to retrieve the VLAN ID of the interface, we can use the `Get-NetAdapaterAdvancedProperty` Cmdlet with the `-DisplayName` flag along with `vlan id`:

```powershell
PS C:\> Get-NetAdapterAdvancedProperty -DisplayName "vlan id"

Name                      DisplayName                    DisplayValue                   RegistryKeyword RegistryValue
----                      -----------                    ------------                   --------------- -------------
Ethernet 2                VLAN ID                        10                                     VLAN_ID               {10}
```

We can also set the VLAN ID of a physical network address using the Set-NetAdapter Cmdlet along with the VlanID flag; this powerful Cmdlet can also be used to customize other properties of interfaces such as MAC addresses:

```powershell
PS C:\> Set-NetAdapter -Name "Ethernet 2" -VlanID 10
```

However, remember that this operation only succeeds if the network interface supports this functionality; otherwise, PowerShell will throw an error indicating that the interface does not support it.
