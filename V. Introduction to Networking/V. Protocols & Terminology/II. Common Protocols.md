# Common Protocols

Internet protocols are standardized rules and guidelines defined in RFCs that specify how devices on a network should communicate with each other. They ensure that devices on a network can exchange information consistently and reliably, regardless of the hardware and software used. For devices to communicate on a network, they need to be connected through a communication channel, such as a wired or wireless connection. The devices then exchange information using a set of standardized protocols that define the format and structure of the data being transmitted. The two main types of connections used on networks are Transmission Control Protocol (TCP) and User Datagram Protocol (UDP).

We need to deal with and know the different and most used protocols. As we have already learned, these protocols are the basis of all communication between our devices and computers in the networks. We have compiled below many of these protocols that we will be dealing with throughout the modules. The better we understand them, the more effectively we can work with them.

### Transmission Control Protocol

TCP is a connection-oriented protocol that establishes a virtual connection between two devices before transmitting data by using a Three-Way-Handshake. This connection is maintained until the data transfer is complete, and the devices can continue to send data back and forth as long as the connection is active.

For example, When we enter a URL into our web browser, the browser sends an HTTP request to the server hosting the website using TCP. The server responds by sending the HTML code for the website back to the browser using TCP. The browser then uses this code to render the website on our screen. This process relies on a TCP connection being established between the browser and the web server and maintained until the data transfer is complete. As a result, TCP is reliable but slower than UDP because it requires additional overhead for establishing and maintaining the connection.

| Protocol                                                  | Acronym    | Port         | Description                                                                                                                                                                        |
| --------------------------------------------------------- | ---------- | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Telnet                                                    | Telnet     | 23           | Remote login service                                                                                                                                                               |
| Secure Shell                                              | SSH        | 22           | Secure remote login service                                                                                                                                                        |
| Simple Network Management Protocol                        | SNMP       | 161-162      | Manage network devices                                                                                                                                                             |
| Hyper Text Transfer Protocol                              | HTTP       | 80           | Used to transfer webpages                                                                                                                                                          |
| Hyper Text Transfer Protocol Secure                       | HTTPS      | 443          | Used to transfer secure webpages                                                                                                                                                   |
| Domain Name System                                        | DNS        | 53           | Lookup domain names                                                                                                                                                                |
| File Transfer Protocol                                    | FTP        | 20-21        | Used to transfer files                                                                                                                                                             |
| Trivial File Transfer Protocol                            | TFTP       | 69           | Used to transfer files                                                                                                                                                             |
| Network Time Protocol                                     | NTP        | 123          | Synchronize computer clocks                                                                                                                                                        |
| Simple Mail Transfer Protocol                             | SMTP       | 25           | Used for email transfer                                                                                                                                                            |
| Post Office Protocol                                      | POP3       | 110          | Used to retrieve emails                                                                                                                                                            |
| Internet Message Access Protocol                          | IMAP       | 143          | Used to access emails                                                                                                                                                              |
| Server Message Block                                      | SMB        | 445          | Used to transfer files                                                                                                                                                             |
| Network File System                                       | NFS        | 111, 2049    | Used to mount remote systems                                                                                                                                                       |
| Bootstrap Protocol                                        | BOOTP      | 67, 68       | Used to bootstrap computers                                                                                                                                                        |
| Kerberos                                                  | Kerberos   | 88           | Used for authentication and authorization                                                                                                                                          |
| Lightweight Directory Access Protocol                     | LDAP       | 389          | Used for directory services                                                                                                                                                        |
| Remote Authentication Dial-In User Service                | RADIUS     | 1812, 1813   | Used for authentication and authorization                                                                                                                                          |
| Dynamic Host Configuration Protocol                       | DHCP       | 67, 68       | Used to configure IP addresses                                                                                                                                                     |
| Remote Desktop Protocol                                   | RDP        | 3389         | Used for remote desktop access                                                                                                                                                     |
| Network News Transfer Protocol                            | NNTP       | 119          | Used to access newsgroups                                                                                                                                                          |
| Remote Procedure Call                                     | RPC        | 135, 137-139 | Used to call remote procedures                                                                                                                                                     |
| Identification Protocol                                   | Ident      | 113          | Used to identify user processes                                                                                                                                                    |
| Internet Control Message Protocol                         | ICMP       | 0-255        | Used to troubleshoot network issues                                                                                                                                                |
| Internet Group Management Protocol                        | IGMP       | 0-255        | Used for multicasting                                                                                                                                                              |
| Oracle DB (Default/Alternative) Listener                  | oracle-tns | 1521/1526    | The Oracle database default/alternative listener is a service that runs on the database host and receives requests from Oracle clients.                                            |
| Ingres Lock                                               | ingreslock | 1524         | Ingres database is commonly used for large commercial applications and as a backdoor that can execute commands remotely via RPC.                                                   |
| Squid Web Proxy                                           | http-proxy | 3128         | Squid web proxy is a caching and forwarding HTTP web proxy used to speed up a web server by caching repeated requests.                                                             |
| Secure Copy Protocol                                      | SCP        | 22           | Securely copy files between systems                                                                                                                                                |
| Session Initiation Protocol                               | SIP        | 5060         | Used for VoIP sessions                                                                                                                                                             |
| Simple Object Access Protocol                             | SOAP       | 80, 443      | Used for web services                                                                                                                                                              |
| Secure Socket Layer                                       | SSL        | 443          | Securely transfer files                                                                                                                                                            |
| TCP Wrappers                                              | TCPW       | 113          | Used for access control                                                                                                                                                            |
| Internet Security Association and Key Management Protocol | ISAKMP     | 500          | Used for VPN connections                                                                                                                                                           |
| Microsoft SQL Server                                      | ms-sql-s   | 1433         | Used for client connections to the Microsoft SQL Server.                                                                                                                           |
| Kerberized Internet Negotiation of Keys                   | KINK       | 892          | Used for authentication and authorization                                                                                                                                          |
| Open Shortest Path First                                  | OSPF       | 89           | Used for routing                                                                                                                                                                   |
| Point-to-Point Tunneling Protocol                         | PPTP       | 1723         | Is used to create VPNs                                                                                                                                                             |
| Remote Execution                                          | REXEC      | 512          | This protocol is used to execute commands on remote computers and send the output of commands back to the local computer.                                                          |
| Remote Login                                              | RLOGIN     | 513          | This protocol starts an interactive shell session on a remote computer.                                                                                                            |
| X Window System                                           | X11        | 6000         | It is a computer software system and network protocol that provides a graphical user interface (GUI) for networked computers.                                                      |
| Relational Database Management System                     | DB2        | 50000        | RDBMS is designed to store, retrieve and manage data in a structured format for enterprise applications such as financial systems, customer relationship management (CRM) systems. |

### User Datagram Protocol

On the other hand, UDP is a connectionless protocol, which means it does not establish a virtual connection before transmitting data. Instead, it sends the data packets to the destination without checking to see if they were received.

For example, when we stream or watch a video on a platform like YouTube, the video data is transmitted to our device using UDP. This is because the video can tolerate some data loss, and the transmission speed is more important than the reliability. If a few packets of video data are lost along the way, it will not significantly impact the overall quality of the video. This makes UDP faster than TCP but less reliable because there is no guarantee that the packets will reach their destination.

| Protocol                            | Acronym    | Port      | Description                                                                                                                                        |
| ----------------------------------- | ---------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Domain Name System                  | DNS        | 53        | It is a protocol to resolve domain names to IP addresses.                                                                                          |
| Trivial File Transfer Protocol      | TFTP       | 69        | It is used to transfer files between systems.                                                                                                      |
| Network Time Protocol               | NTP        | 123       | It synchronizes computer clocks in a network.                                                                                                      |
| Simple Network Management Protocol  | SNMP       | 161       | It monitors and manages network devices remotely.                                                                                                  |
| Routing Information Protocol        | RIP        | 520       | It is used to exchange routing information between routers.                                                                                        |
| Internet Key Exchange               | IKE        | 500       | Internet Key Exchange                                                                                                                              |
| Bootstrap Protocol                  | BOOTP      | 68        | It is used to bootstrap hosts in a network.                                                                                                        |
| Dynamic Host Configuration Protocol | DHCP       | 67        | It is used to assign IP addresses to devices in a network dynamically.                                                                             |
| Telnet                              | TELNET     | 23        | It is a text-based remote access communication protocol.                                                                                           |
| MySQL                               | MySQL      | 3306      | It is an open-source database management system.                                                                                                   |
| Terminal Server                     | TS         | 3389      | It is a remote access protocol used for Microsoft Windows Terminal Services by default.                                                            |
| NetBIOS Name                        | netbios-ns | 137       | It is used in Windows operating systems to resolve NetBIOS names to IP addresses on a LAN.                                                         |
| Microsoft SQL Server                | ms-sql-m   | 1434      | Used for the Microsoft SQL Server Browser service.                                                                                                 |
| Universal Plug and Play             | UPnP       | 1900      | It is a protocol for devices to discover each other on the network and communicate.                                                                |
| PostgreSQL                          | PGSQL      | 5432      | It is an object-relational database management system.                                                                                             |
| Virtual Network Computing           | VNC        | 5900      | It is a graphical desktop sharing system.                                                                                                          |
| X Window System                     | X11        | 6000-6063 | It is a computer software system and network protocol that provides GUI on Unix-like systems.                                                      |
| Syslog                              | SYSLOG     | 514       | It is a standard protocol to collect and store log messages on a computer system.                                                                  |
| Internet Relay Chat                 | IRC        | 194       | It is a real-time Internet text messaging (chat) or synchronous communication protocol.                                                            |
| OpenPGP                             | OpenPGP    | 11371     | It is a protocol for encrypting and signing data and communications.                                                                               |
| Internet Protocol Security          | IPsec      | 500       | IPsec is also a protocol that provides secure, encrypted communication. It is commonly used in VPNs to create a secure tunnel between two devices. |
| Internet Key Exchange               | IKE        | 11371     | It is a protocol for encrypting and signing data and communications.                                                                               |
| X Display Manager Control Protocol  | XDMCP      | 177       | XDMCP is a network protocol that allows a user to remotely log in to a computer running the X11.                                                   |

### ICMP

Internet Control Message Protocol (ICMP) is a protocol used by devices to communicate with each other on the Internet for various purposes, including error reporting and status information. It sends requests and messages between devices, which can be used to report errors or provide status information.

#### ICMP Requests

A request is a message sent by one device to another to request information or perform a specific action. An example of a request in ICMP is the ping request, which tests the connectivity between two devices. When one device sends a ping request to another, the second device responds with a ping reply message.

#### ICMP Messages

A message in ICMP can be either a request or a reply. In addition to ping requests and responses, ICMP supports other types of messages, such as error messages, destination unreachable, and time exceeded messages. These messages are used to communicate various types of information and errors between devices on the network.

For example, if a device tries to send a packet to another device and the packet cannot be delivered, the device can use ICMP to send an error message back to the sender. ICMP has two different versions:

- ICMPv4: For IPv4 only
- ICMPv6: For IPv6 only

ICMPv4 is the original version of ICMP, developed for use with IPv4. It is still widely used and is the most common version of ICMP. On the other hand, ICMPv6 was developed for IPv6. It includes additional functionality and is designed to address some of the limitations of ICMPv4.

| Request Type         | Description                                                                                                                                                                                                                                       |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Echo Request         | This message tests whether a device is reachable on the network. When a device sends an echo request, it expects to receive an echo reply message. For example, the tools tracert (Windows) or traceroute (Linux) always send ICMP echo requests. |
| Timestamp Request    | This message determines the time on a remote device.                                                                                                                                                                                              |
| Address Mask Request | This message is used to request the subnet mask of a device.                                                                                                                                                                                      |

| Message Type            | Description                                                                                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Echo reply              | This message is sent in response to an echo request message.                                                                     |
| Destination unreachable | This message is sent when a device cannot deliver a packet to its destination.                                                   |
| Redirect                | A router sends this message to inform a device that it should send its packets to a different router.                            |
| time exceeded           | This message is sent when a packet has taken too long to reach its destination.                                                  |
| Parameter problem       | This message is sent when there is a problem with a packet's header.                                                             |
| Source quench           | This message is sent when a device receives packets too quickly and cannot keep up. It is used to slow down the flow of packets. |

Another crucial part of ICMP for us is the Time-To-Live (TTL) field in the ICMP packet header that limits the packet's lifetime as it travels through the network. It prevents packets from circulating indefinitely on the network in the event of routing loops. Each time a packet passes through a router, the router decrements the TTL value by 1. When the TTL value reaches 0, the router discards the packet and sends an ICMP Time Exceeded message back to the sender.

We can also use TTL to determine the number of hops a packet has taken and the approximate distance to the destination. For example, if a packet has a TTL of 10 and takes 5 hops to reach its destination, it can be inferred that the destination is approximately 5 hops away. For example, if we see a ping with the TTL value of 122, it could mean that we are dealing with a Windows system (TTL 128 by default) that is 6 hops away.

However, it is also possible to guess the operating system based on the default TTL value used by the device. Each operating system typically has a default TTL value when sending packets. This value is set in the packet's header and is decremented by 1 each time the packet passes through a router. Therefore, examining a device's default TTL value makes it possible to infer which operating system the device is using. For example: Windows systems (2000/XP/2003/Vista/10) typically have a default TTL value of 128, while macOS and Linux systems typically have a default TTL value of 64 and Solaris' default TTL value of 255. However, it is important to note that the user can change these values, so they should be independent of a definitive way to determine a device's operating system.

### VoIP

Voice over Internet Protocol (VoIP) is a method of transmitting voice and multimedia communications. For example, it allows us to make phone calls using a broadband internet connection instead of a traditional phone line, like Skype, Whatsapp, Google Hangouts, Slack, Zoom, and others.

The most common VoIP ports are TCP/5060 and TCP/5061, which are used for the Session Initiation Protocol (SIP). However, the port TCP/1720 may also be used by some VoIP systems for the H.323 protocol, a set of standards for multimedia communication over packet-based networks. Still, SIP is more widely used than H.323 in VoIP systems.

Nevertheless, SIP is a signaling protocol for initiating, maintaining, modifying, and terminating real-time sessions involving video, voice, messaging, and other communications applications and services between two or more endpoints on the Internet. Therefore, it uses requests and methods between the endpoints. The most common SIP requests and methods are:

| Method   | Description                                                                                                        |
| -------- | ------------------------------------------------------------------------------------------------------------------ |
| INVITE   | Initiates a session or invites another endpoint to participate.                                                    |
| ACK      | Confirms the receipt of an INVITE request.                                                                         |
| BYE      | Terminate a session.                                                                                               |
| CANCEL   | Cancels a pending INVITE request.                                                                                  |
| REGISTER | Registers a SIP user agent (UA) with a SIP server.                                                                 |
| OPTIONS  | Requests information about the capabilities of a SIP server or user agent, such as the types of media it supports. |

#### Information Disclosure

However, SIP allows us to enumerate existing users for potential attacks. This can be done for various purposes, such as determining a user's availability, finding out information about the user's capabilities or services, or performing brute-force attacks on user accounts later on.

One of the possible ways to enumerate users is the SIP OPTIONS request. It is a method used to request information about the capabilities of a SIP server or user agents, such as the types of media it supports, the codecs it can decode, and other details. The OPTIONS request can probe a SIP server or user agent for information or test its connectivity and availability.

During our analysis, it is possible to discover a SEPxxxx.cnf file, where xxxx is a unique identifier, is a configuration file used by Cisco Unified Communications Manager, formerly known as Cisco CallManager, to define the settings and parameters for a Cisco Unified IP Phone. The file specifies the phone model, firmware version, network settings, and other details.
