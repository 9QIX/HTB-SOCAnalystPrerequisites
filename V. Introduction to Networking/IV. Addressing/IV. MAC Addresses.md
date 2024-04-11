# MAC Addresses

Each host in a network has its own 48-bit (6 octets) Media Access Control (MAC) address, represented in hexadecimal format. MAC is the physical address for our network interfaces. There are several different standards for the MAC address:

- Ethernet (IEEE 802.3)
- Bluetooth (IEEE 802.15)
- WLAN (IEEE 802.11)

This is because the MAC address addresses the physical connection (network card, Bluetooth, or WLAN adapter) of a host. Each network card has its individual MAC address, which is configured once on the manufacturer's hardware side but can always be changed, at least temporarily.

Let's have a look at an example of such a MAC address:

MAC address:

```
DE:AD:BE:EF:13:37
DE-AD-BE-EF-13-37
DEAD.BEEF.1337
```

| Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet |
| -------------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Binary         | 1101 1110 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111 |
| Hex            | DE        | AD        | BE        | EF        | 13        | 37        |

When an IP packet is delivered, it must be addressed on layer 2 to the destination host's physical address or to the router / NAT, which is responsible for routing. Each packet has a sender address and a destination address.

The MAC address consists of a total of 6 bytes. The first half (3 bytes / 24 bit) is the so-called Organization Unique Identifier (OUI) defined by the Institute of Electrical and Electronics Engineers (IEEE) for the respective manufacturers.

| Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet |
| -------------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Binary         | 1101 1110 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111 |
| Hex            | DE        | AD        | BE        | EF        | 13        | 37        |

The last half of the MAC address is called the Individual Address Part or Network Interface Controller (NIC), which the manufacturers assign. The manufacturer sets this bit sequence only once and thus ensures that the complete address is unique.

| Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet |
| -------------- | --------- | --------- | --------- | --------- | --------- | --------- |
| Binary         | 1101 1110 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111 |
| Hex            | DE        | AD        | BE        | EF        | 13        | 37        |

If a host with the IP target address is located in the same subnet, the delivery is made directly to the target computer's physical address. However, if this host belongs to a different subnet, the Ethernet frame is addressed to the MAC address of the responsible router (default gateway). If the Ethernet frame's destination address matches its own layer 2 address, the router will forward the frame to the higher layers. Address Resolution Protocol (ARP) is used in IPv4 to determine the MAC addresses associated with the IP addresses.

As with IPv4 addresses, there are also certain reserved areas for the MAC address. These include, for example, the local range for the MAC.

Local Range

- 02:00:00:00:00:00
- 06:00:00:00:00:00
- 0A:00:00:00:00:00
- 0E:00:00:00:00:00

Furthermore, the last two bits in the first octet can play another essential role. The last bit can have two states, 0 and 1, as we already know. The last bit identifies the MAC address as Unicast (0) or Multicast (1). With unicast, it means that the packet sent will reach only one specific host.

MAC Unicast
Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet
--- | --- | --- | --- | --- | --- | ---
Binary | 1101 1110 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111
Hex | DE | AD | BE | EF | 13 | 37

With multicast, the packet is sent only once to all hosts on the local network, which then decides whether or not to accept the packet based on their configuration. The multicast address is a unique address, just like the broadcast address, which has fixed octet values. Broadcast in a network represents a broadcasted call, where data packets are transmitted simultaneously from one point to all members of a network. It is mainly used if the address of the receiver of the packet is not yet known. An example is the ARP (for MAC addresses) and DHCP (for IPv4 addresses) protocols.

The defined values of each octet are marked green.

MAC Multicast
Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet
--- | --- | --- | --- | --- | --- | ---
Binary | 0000 0001 | 0000 0000 | 0101 1110 | 1110 1111 | 0001 0011 | 0011 0111
Hex | 01 | 00 | 5E | EF | 13 | 37

MAC Broadcast
Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet
--- | --- | --- | --- | --- | --- | ---
Binary | 1111 1111 | 1111 1111 | 1111 1111 | 1111 1111 | 1111 1111 | 1111 1111
Hex | FF | FF | FF | FF | FF | FF

The second last bit in the first octet identifies whether it is a global OUI, defined by the IEEE, or a locally administrated MAC address.

Global OUI
Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet
--- | --- | --- | --- | --- | --- | ---
Binary | 1101 1100 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111
Hex | DC | AD | BE | EF | 13 | 37

Locally Administrated
Representation | 1st Octet | 2nd Octet | 3rd Octet | 4th Octet | 5th Octet | 6th Octet
--- | --- | --- | --- | --- | --- | ---
Binary | 1101 1110 | 1010 1101 | 1011 1110 | 1110 1111 | 0001 0011 | 0011 0111
Hex | DE | AD | BE | EF | 13 | 37

### Address Resolution Protocol

MAC addresses can be changed/manipulated or spoofed, and as such, they should not be relied upon as a sole means of security or identification. Network administrators should implement additional security measures, such as network segmentation and strong authentication protocols, to protect against potential attacks.

There exist several attack vectors that can potentially be exploited through the use of MAC addresses:

- MAC spoofing: This involves altering the MAC address of a device to match that of another device, typically to gain unauthorized access to a network.
- MAC flooding: This involves sending many packets with different MAC addresses to a network switch, causing it to reach its MAC address table capacity and effectively preventing it from functioning correctly.
- MAC address filtering: Some networks may be configured only to allow access to devices with specific MAC addresses that we could potentially exploit by attempting to gain access to the network using a spoofed MAC address.

**Address Resolution Protocol**

Address Resolution Protocol (ARP) is a network protocol. It is an important part of the network communication used to resolve a network layer (layer 3) IP address to a link layer (layer 2) MAC address. It maps a host's IP address to its corresponding MAC address to facilitate communication between devices on a Local Area Network (LAN). When a device on a LAN wants to communicate with another device, it sends a broadcast message containing the destination IP address and its own MAC address. The device with the matching IP address responds with its own MAC address, and the two devices can then communicate directly using their MAC addresses. This process is known as ARP resolution.

ARP is an important part of the network communication process because it allows devices to send and receive data using MAC addresses rather than IP addresses, which can be more efficient. Two types of request messages can be used:

**ARP Request**
When a device wants to communicate with another device on a LAN, it sends an ARP request to resolve the destination device's IP address to its MAC address. The request is broadcast to all devices on the LAN and contains the IP address of the destination device. The device with the matching IP address responds with its MAC address.

**ARP Reply**
When a device receives an ARP request, it sends an ARP reply to the requesting device with its MAC address. The reply message contains the IP and MAC addresses of both the requesting and the responding devices.

**Tshark Capture of ARP Requests**

```
  MAC Addresses
1   0.000000 10.129.12.100 -> 10.129.12.255 ARP 60  Who has 10.129.12.101?  Tell 10.129.12.100
2   0.000015 10.129.12.101 -> 10.129.12.100 ARP 60  10.129.12.101 is at AA:AA:AA:AA:AA:AA

3   0.000030 10.129.12.102 -> 10.129.12.255 ARP 60  Who has 10.129.12.103?  Tell 10.129.12.102
4   0.000045 10.129.12.103 -> 10.129.12.102 ARP 60  10.129.12.103 is at BB:BB:BB:BB:BB:BB
```

The "who has" message in the first and third lines indicates that a device is requesting the MAC address for the specified IP address, while the second and fourth lines show the ARP reply with the MAC address of the destination device.

However, it is also vulnerable to attacks, such as ARP Spoofing, which can be used to intercept or manipulate traffic on the network. However, to protect against such attacks, it is important to implement security measures such as firewalls and intrusion detection systems.

**ARP spoofing**

ARP spoofing, also known as ARP cache poisoning or ARP poison routing, is an attack that can be done using tools like Ettercap or Cain & Abel in which we send falsified ARP messages over a LAN. The goal is to associate our MAC address with the IP address of a legitimate device on the company's network, effectively allowing us to intercept traffic intended for the legitimate device. For example, this could look like the following:

```
  MAC Addresses
1   0.000000 10.129.12.100 -> 10.129.12.101 ARP 60  10.129.12.100 is at AA:AA:AA:AA:AA:AA
2   0.000015 10.129.12.100 -> 10.129.12.255 ARP 60  Who has 10.129.12.101?  Tell 10.129.12.100
3   0.000030 10.129.12.101 -> 10.129.12.100 ARP 60  10.129.12.101 is at BB:BB:BB:BB:BB:BB
4   0.000045 10.129.12.100 -> 10.129.12.101 ARP 60  10.129.12.100 is at AA:AA:AA:AA:AA:AA
```

The first and fourth lines show us (10.129.12.100) sending falsified ARP messages to the target, associating its MAC address with its IP address (10.129.12.101). The second and third lines show the target sending an ARP request and replying to our MAC address. This indicates that we have poisoned the target's ARP cache and that all traffic intended for the target will now be sent to our MAC address.

We can use ARP poisoning to perform various activities, such as stealing sensitive information, redirecting traffic, or launching MITM attacks. However, to protect against ARP spoofing, it is important to use secure network protocols, such as IPSec or SSL, and to implement security measures, such as firewalls and intrusion detection systems.
