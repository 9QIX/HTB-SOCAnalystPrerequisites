# Wireless Networks

Wireless networks are computer networks that use wireless data connections between network nodes. These networks allow devices such as laptops, smartphones, and tablets to communicate with each other and the Internet without needing physical connections such as cables.

Wireless networks use radio frequency (RF) technology to transmit data between devices. Each device on a wireless network has a wireless adapter that converts data into RF signals and sends them over the air. Other devices on the network receive these signals with their own wireless adapters, and the data is then converted back into a usable form. Those can operate over various ranges, depending on the technology used. For example, a local area network (LAN) that covers a small area, such as a home or small office, might use a wireless technology called WiFi, which has a range of a few hundred feet. On the other hand, a wireless wide area network (WWAN) might use mobile telecommunication technology such as cellular data (3G, 4G LTE, 5G), which can cover a much larger area, such as an entire city or region.

Therefore, to connect to a wireless network, a device must be within range of the network and configured with the correct network settings, such as the network name and password. Once connected, devices can communicate with each other and the Internet, allowing users to access online resources and exchange data.

Communication between devices occurs over RF in the 2.4 GHz or 5 GHz bands in a WiFi network. When a device, like a laptop, wants to send data over the network, it first communicates with the Wireless Access Point (WAP) to request permission to transmit. The WAP is a central device, like a router, that connects the wireless network to a wired network and controls access to the network. Once the WAP grants permission, the transmitting device sends the data as RF signals, which are received by the wireless adapters of other devices on the network. The data is then converted back into a usable form and passed on to the appropriate application or system.

The strength of the RF signal and the distance it can travel are influenced by factors such as the transmitter's power, the presence of obstacles, and the density of RF noise in the environment. So, to ensure reliable communication, WiFi networks use techniques such as spread spectrum transmission and error correction to overcome these challenges.

## WiFi Connection

The device must also be configured with the correct network settings, such as the network name / Service Set Identifier (SSID) and password. So, to connect to the router, the laptop uses a wireless networking protocol called IEEE 802.11. This protocol defines the technical details of how wireless devices communicate with each other and with WAPs. When a device wants to join a WiFi network, it sends a request to the WAP to initiate the connection process. This request is known as a connection request frame or association request and is sent using the IEEE 802.11 wireless networking protocol. The connection request frame contains various fields of information, including the following but not limited to:

- MAC address: A unique identifier for the device's wireless adapter.
- SSID: The network name, also known as the Service Set Identifier of the WiFi network.
- Supported data rates: A list of the data rates the device can communicate.
- Supported channels: A list of the channels (frequencies) on which the device can communicate.
- Supported security protocols: A list of the security protocols that the device is capable of using, such as WPA2/WPA3.

The device then uses this information to configure its wireless adapter and connect to the WAP. Once the connection is established, the device can communicate with the WAP and other network devices. It can also access the Internet and other online resources through the WAP, which acts as a gateway to the wired network. However, the SSID can be hidden by disabling broadcasting. That means that devices that search for that specific WAP will not be able to identify its SSID. Nevertheless, the SSID can still be found in the authentication packet.

In addition to the IEEE 802.11 protocol, other networking protocols and technologies may also be used, like TCP/IP, DHCP, and WPA2, in a WiFi network to perform tasks such as assigning IP addresses to devices, routing traffic between devices, and providing security.

## WEP Challenge-Response Handshake

The challenge-response handshake is a process to establish a secure connection between a WAP and a client device in a wireless network that uses the WEP security protocol. This involves exchanging packets between the WAP and the client device to authenticate the device and establish a secure connection.

| Step | Who    | Description                                                                                                                                  |
| ---- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Client | Sends an association request packet to the WAP, requesting access.                                                                           |
| 2    | WAP    | Responds with an association response packet to the client, which includes a challenge string.                                               |
| 3    | Client | Calculates a response to the challenge string and a shared secret key and sends it back to the WAP.                                          |
| 4    | WAP    | Calculates the expected response to the challenge with the same shared secret key and sends an authentication response packet to the client. |

Nevertheless, some packets can get lost, so the so-called CRC checksum has been integrated. Cyclic Redundancy Check (CRC) is an error-detection mechanism used in the WEP protocol to protect against data corruption in wireless communications. A CRC value is calculated for each packet transmitted over the wireless network based on the packet's data. It is used to verify the integrity of the data. When the destination device receives the packet, the CRC value is recalculated and compared to the original value. If the values match, the data has been transmitted successfully without any errors. However, if the values do not match, the data has been corrupted and needs to be retransmitted.

The design of the CRC mechanism has a flaw that allows us to decrypt a single packet without knowing the encryption key. This is because the CRC value is calculated using the plaintext data in the packet rather than the encrypted data. In WEP, the CRC value is included in the packet header along with the encrypted data. When the destination device receives the packet, the CRC value is recalculated and compared to the original one to ensure that the data has been transmitted successfully without any errors. However, we can use the CRC to determine the plaintext data in the packet, even if the data is encrypted.

## Security Features

WiFi networks have several security features to protect against unauthorized access and ensure the privacy and integrity of data transmitted over the network. Some of the leading security features include but are not limited to:

- Encryption
- Access Control
- Firewall

### Encryption

We can use various encryption algorithms to protect the confidentiality of data transmitted over wireless networks. The most common encryption algorithms in WiFi networks are Wired Equivalent Privacy (WEP), WiFi Protected Access 2 (WPA2), and WiFi Protected Access 3 (WPA3).

### Access Control

WiFi networks are configured by default to allow authorized devices to join the network using specific authentication methods. However, these methods can be changed by requiring a password or a unique identifier (such as a MAC address) to identify authorized devices.

### Firewall

A firewall is a security system that controls incoming and outgoing network traffic based on predetermined security rules. For example, WiFi routers often have built-in firewalls that can block incoming traffic from the Internet and protect against various types of cyber threats.

## Encryption Protocols

Wired Equivalent Privacy (WEP) and WiFi Protected Access (WPA) are encryption protocols that secure data transmitted over a WiFi network. WPA can use different encryption algorithms, including Advanced Encryption Standard (AES).

### WEP

WEP uses a 40-bit or 104-bit key to encrypt data, while WPA using AES uses a 128-bit key. Longer keys provide more robust encryption and are more resistant to attacks. However, it is vulnerable to various attacks that can allow an attacker to decrypt data transmitted over the network. In addition, WEP is not compatible with newer devices and operating systems and is generally no longer considered secure. Finally, WEP uses the RC4 cipher encryption algorithm, which makes it vulnerable to attacks.

However, WEP uses a shared key for authentication, which means the same key is used for encryption and authentication. There are two versions of the WEP protocol:

- WEP-40/WEP-64
- WEP-104

WEP-40, also known as WEP-64, uses a 40-bit (secret) key, while WEP-104 uses a 104-bit key. The key is divided into an Initialization Vector (IV) and a secret key.

The IV is a small value included in the packet header along with the encrypted data and is used to create the key for both WEP-40 and WEP-104 and is included to ensure that each key is unique. The secret key is a series of random bits used to encrypt the data. However, the WEP-104 has a 80-bits long secret key.

### WPA

WPA provides the highest level of security and is not susceptible to the same types of attacks as WEP. In addition, WPA uses more secure authentication methods, such as a Pre-Shared Key (PSK) or an 802.1X authentication server, which provide stronger protection against unauthorized access. Although older devices may not support WPA is compatible with most devices and operating systems. All wireless networks, especially in critical infrastructure like offices, should generally implement at least WPA2 or even WPA3 encryption.

## Authentication Protocols

Lightweight Extensible Authentication Protocol (LEAP) and Protected Extensible Authentication Protocol (PEAP) are authentication protocols used to secure wireless networks to provide a secure method for authenticating devices on a wireless network and are often used in conjunction with WEP or WPA to provide an additional layer of security.

LEAP and PEAP are both based on the Extensible Authentication Protocol (EAP), a framework for authentication used in various networking contexts. However, one key difference between LEAP and PEAP is how they secure the authentication process.

LEAP uses a shared key for authentication, which means that the same key is used for encryption and authentication. This can make it relatively easy for us to gain access to the network if the key is compromised.

However, PEAP uses a more secure authentication method called tunneled Transport Layer Security (TLS). This method establishes a secure connection between the device and the WAP using a digital certificate, and an encrypted tunnel protects the authentication process. This provides more robust protection against unauthorized access and is more resistant to attacks.

## TACACS+

In a wireless network, when a wireless access point (WAP) sends an authentication request to a Terminal Access Controller Access-Control System Plus (TACACS+) server, it is likely that the entire request packet will be encrypted to protect the confidentiality and integrity of the request.

TACACS+ is a protocol used to authenticate and authorize users accessing network devices, such as routers and switches. When a WAP sends an authentication request to a TACACS+ server, the request typically includes the user's credentials and other information about the session.

Encrypting the authentication request helps to ensure that this sensitive information is not visible to unauthorized parties who may be able to intercept the request. At the same time, it is being transmitted over the network. It also helps prevent tampering with the request or replacing it with a malicious request of their own.

Several encryption methods may be used to encrypt the authentication request, such as SSL/TLS or IPSec. The specific encryption method used may depend on the configuration of the TACACS+ server and the capabilities of the WAP.

## Disassociation Attack

A Disassociation Attack is a type of all wireless network attack that aims to disrupt the communication between a WAP and its clients by sending disassociation frames to one or more clients.

The WAP uses disassociation frames to disconnect a client from the network. When a WAP sends a disassociation frame to a client, the client will disconnect from the network and have to reconnect to continue using the network.

We can launch the attack from within or outside the network depending on our location and network security measures. The purpose of this attack is to disrupt the communication between the WAP and its clients, causing the clients to disconnect and possibly causing inconvenience or disruption to the users. We can also use it as a precursor to other attacks, such as a MITM attack, by forcing the clients to reconnect to the network and potentially exposing them to further attacks.

## Wireless Hardening

There are many different ways to protect wireless networks. However, some examples should be considered to increase wireless networks' security dramatically. These are the following, but not limited to:

- Disabling broadcasting
- WiFi Protected Access
- MAC filtering
- Deploying EAP-TLS

### Disabling Broadcasting

Disabling the broadcasting of the SSID is a security measure that can help harden a WAP by making it more difficult to discover and connect to the network. When the SSID is broadcasted, it is included in beacon frames regularly transmitted by the WAP to advertise the availability of the network. By disabling the broadcasting of the SSID, the WAP will not transmit beacon frames, and the network will not be visible to devices that are not already connected to the network.

### WPA

Again, WPA provides strong encryption and authentication for wireless communications, helping protect against unauthorized network access and sensitive data interception. WPA includes two main versions:

- WPA-Personal
- WPA-Enterprise

WPA-Personal, designed for home and small business networks, and WPA-Enterprise, designed for larger organizations and uses a centralized authentication server (e.g., RADIUS or TACACS+) to verify the identity of clients.

### MAC Filtering

MAC filtering is a security measure that allows a WAP to accept or reject connections from specific devices based on their MAC addresses. By configuring the WAP to accept connections only from devices with approved MAC addresses, it is possible to prevent unauthorized devices from connecting to the network.

### Deploying EAP-TLS

EAP-TLS is a security protocol used to authenticate and encrypt wireless communications. It uses digital certificates and PKI to verify the identity of clients and establish secure connections. Deploying EAP-TLS can help to harden a WAP by providing strong authentication and encryption for wireless communications, which can protect against unauthorized access to the network and the interception of sensitive data.
