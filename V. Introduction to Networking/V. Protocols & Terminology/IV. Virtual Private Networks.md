## Virtual Private Networks

A Virtual Private Network (VPN) is a technology that allows a secure and encrypted connection between a private network and a remote device. This allows the remote machine to access the private network directly, providing secure and confidential access to the network's resources and services. For example, an administrator from another location has to manage the internal servers so that the employees can continue to use the internal services. Many companies limit servers' access, so clients can only reach those servers from the local network. This is where VPN comes into play, where the administrator connects to the VPN server via the internet, authenticates himself, and thus creates an encrypted tunnel so that others cannot read the data transfer. In addition, the administrator's computer is also assigned a local (internal) IP address through which he can access and manage the internal servers. Administrators commonly use VPNs to provide secure and cost-effective remote access to a company's network. VPN typically uses the ports TCP/1723 for Point-to-Point Tunneling Protocol PPTP VPN connections and UDP/500 for IKEv1 and IKEv2 VPN connections.

This allows employees to access the network and its resources, such as email and file servers, from remote locations, such as their homes or while traveling. There are several reasons why administrators use VPNs. VPNs encrypt the connection between the remote device and the private network, making it much more difficult for attackers to intercept and steal sensitive information. With this, the entire communication is more secure.

Another reason is that VPNs allow employees to access the private network and its resources remotely from anywhere, as long as they have an internet connection. This is particularly useful for employees who need to work remotely, such as those traveling or working from home. Additionally, VPNs can be more cost-effective than other remote access solutions, such as leased lines or dedicated connections, because they use the public internet to connect remote users to the private network.

Moreover, we can use VPNs to connect multiple remote locations, such as branch offices, into a single private network, making it easier to manage and access network resources. However, several components and requirements are necessary for a VPN to work:

| Requirement    | Description                                                                                                                                                             |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| VPN Client     | This is installed on the remote device and is used to establish and maintain a VPN connection with the VPN server. For example, this could be an OpenVPN client.        |
| VPN Server     | This is a computer or network device responsible for accepting VPN connections from VPN clients and routing traffic between the VPN clients and the private network.    |
| Encryption     | VPN connections are encrypted using a variety of encryption algorithms and protocols, such as AES and IPsec, to secure the connection and protect the transmitted data. |
| Authentication | The VPN server and client must authenticate each other using a shared secret, certificate, or another authentication method to establish a secure connection.           |

The VPN client and server use these ports to establish and maintain the VPN connection. At the TCP/IP layer, a VPN connection typically uses the Encapsulating Security Payload (ESP) protocol to encrypt and authenticate the VPN traffic. This allows the VPN client and server to exchange data over the public internet securely.

## IPsec

Internet Protocol Security (IPsec) is a network security protocol that provides encryption and authentication for internet communications. It is a powerful and widely-used security protocol that provides encryption and authentication for internet communications and works by encrypting the data payload of each IP packet and adding an authentication header (AH), which is used to verify the integrity and authenticity of the packet. IPsec uses a combination of two protocols to provide encryption and authentication:

1. **Authentication Header (AH)**: This protocol provides integrity and authenticity for IP packets but does not provide encryption. It adds an authentication header to each IP packet, which contains a cryptographic checksum that can be used to verify that the packet has not been tampered with.
2. **Encapsulating Security Payload (ESP)**: This protocol provides encryption and optional authentication for IP packets. It encrypts the data payload of each IP packet and optionally adds an authentication header, similar to AH.

IPsec can be used in two modes:

| Mode           | Description                                                                                                                                                                                        |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Transport Mode | In this mode, IPsec encrypts and authenticates the data payload of each IP packet but does not encrypt the IP header. This is typically used to secure end-to-end communication between two hosts. |
| Tunnel Mode    | With this mode, IPsec encrypts and authenticates the entire IP packet, including the IP header. This is typically used to create a VPN tunnel between two networks.                                |

For example, an administrator could place a firewall in between. In order to facilitate IPsec VPN traffic from a VPN client outside a firewall to a VPN server inside, the firewall would need to allow the following protocols:

| Protocol                             | Port      | Description                                                                                                                                                                                                                                                                                              |
| ------------------------------------ | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Internet Protocol (IP)               | UDP/50-51 | This is the primary protocol that provides the foundation for all internet communication. It is used to route packets of data between the VPN client and the VPN server.                                                                                                                                 |
| Internet Key Exchange (IKE)          | UDP/500   | IKE is a protocol that is used to establish and maintain secure communication between the VPN client and the VPN server. It is based on the Diffie-Hellman key exchange algorithm, and it is used to negotiate and establish shared secret keys that can be used to encrypt and decrypt the VPN traffic. |
| Encapsulating Security Payload (ESP) | UDP/4500  | ESP is also a protocol that provides encryption and authentication for IP datagrams. It is used to encrypt the VPN traffic between the VPN client and the VPN server, using the keys that were negotiated with IKE.                                                                                      |

These protocols are necessary for facilitating IPsec VPN traffic because they provide the security and encryption that are required for secure communication over the public internet. Without these protocols, the VPN traffic would be vulnerable to interception and tampering.

## PPTP

Point-to-Point Tunneling Protocol (PPTP) is also a network protocol that allows the creation of VPNs and works by establishing a secure tunnel between the VPN client and server and then encapsulating the data being transmitted within this tunnel.

It is an extension of the PPTP and is implemented in many operating systems. Due to known vulnerabilities, PPTP is no longer considered secure today. PPTP can be used to tunnel protocols such as IP, IPX, or NetBEUI via IP. Due to known vulnerabilities, the PPTP is no longer considered secure and has been largely replaced by other VPN protocols such as L2TP/IPsec, IPsec/IKEv2, or OpenVPN. Since 2012, however, PPTP is no longer considered secure because the authentication method MSCHAPv2 uses the dusty DES and can therefore be easily cracked with special hardware.
