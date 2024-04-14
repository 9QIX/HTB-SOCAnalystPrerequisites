# TCP/UDP Connections

Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) are both protocols used in information and data transmission on the Internet. Typically, TCP connections transmit important data, such as web pages and emails. In contrast, UDP connections transmit real-time data such as streaming video or online gaming.

TCP is a connection-oriented protocol that ensures that all data sent from one computer to another is received. It is like a telephone conversation where both parties remain connected until the call is terminated. If an error occurs while sending data, the receiver sends a message back so the sender can resend the missing data. This makes TCP reliable and slower than UDP because more time is required for transmission and error recovery.

UDP, on the other hand, is a connectionless protocol. It is used when speed is more important than reliability, such as for video streaming or online gaming. With UDP, there is no verification that the received data is complete and error-free. If an error occurs while sending data, the receiver will not receive this missing data, and no message will be sent back to resend it. Some data may be lost with UDP, but the overall transmission is faster.

## IP Packet

An Internet Protocol (IP) packet is the data area used by the network layer of the Open Systems Interconnection (OSI) model to transmit data from one computer to another. It consists of a header and the payload, the actual payload data.

We can also think of the IP packet as a letter sent in an envelope. The envelope contains the header, which includes information on the sender and the recipient, as well as instructions for routing the letter, i.e., via which post offices the letter should be sent. The letter itself is the payload, the actual payload data.

### IP header

The header of an IP packet contains several fields that have important information.

| Field                  | Description                                                                    |
| ---------------------- | ------------------------------------------------------------------------------ |
| Version                | Indicates which version of the IP protocol is being used                       |
| Internet Header Length | Indicates the size of the header in 32-bit words                               |
| Class of Service       | Means how important the transmission of the data is                            |
| Total length           | Specifies the total length of the packet in bytes                              |
| Identification (ID)    | Is used to identify fragments of the packet when fragmented into smaller parts |
| Flags                  | Used to indicate fragmentation                                                 |
| Fragment Offset        | Indicates where the current fragment is placed in the packet                   |
| Time to Live           | Specifies how long the packet may remain on the network                        |
| Protocol               | Specifies which protocol is used to transmit the data, such as TCP or UDP      |
| Checksum               | Is used to detect errors in the header                                         |
| Source/Destination     | Indicate where the packet was sent from and where it is being sent to          |
| Options                | Contain optional information for routing                                       |
| Padding                | Pads the packet to a full word length                                          |

We may see a computer with multiple IP addresses in different networks. Here we should pay attention to the IP ID field. It is used to identify fragments of an IP packet when fragmented into smaller parts. It is a 16-bit field with a unique number ranging from 0-65535.

If a computer has multiple IP addresses, the IP ID field will be different for each packet sent from the computer but very similar. In TCPdump, the network traffic might look something like this:

```
Network Sniffing
  TCP/UDP Connections
IP 10.129.1.100.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1337
IP 10.129.1.100.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1338
IP 10.129.1.100.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1339
IP 10.129.2.200.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1340
IP 10.129.2.200.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1341
IP 10.129.2.200.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1342
```

We can see from the output that two different IP addresses are sending packets to IP address 10.129.1.1. However, from the IP ID, we can see that the packets are continuous. This strongly indicates that the two IP addresses belong to the same host in the network.

### IP Record-Route Field

The Record-Route field in the IP header also records the route to a destination device. When the destination device sends back the ICMP Echo Reply packet, the IP addresses of all devices that pass through the packet are listed in the Record-Route field of the IP header. This happens when we use the following command, for example:

```
  TCP/UDP Connections
z0x9n@htb[/htb]$ ping -c 1 -R 10.129.143.158

PING 10.129.143.158 (10.129.143.158) 56(124) bytes of data.
64 bytes from 10.129.143.158: icmp_seq=1 ttl=63 time=11.7 ms
RR: 10.10.14.38
        10.129.0.1
        10.129.143.158
        10.129.143.158
        10.10.14.1
        10.10.14.38


--- 10.129.143.158 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 11.688/11.688/11.688/0.000 ms
```

The output indicates that a ping request was sent and a response was received from the destination device and also shows the Record-Route field in the IP header of the ICMP Echo Request packet. The Record Route field contains the IP addresses of all devices that passed through the ICMP Echo Request packet on the way to the destination device. In this case, the Record-Route field contains the IP addresses:

```
10.10.14.38    10.129.0.1    10.129.143.158
10.129.143.158    10.10.14.1    10.10.14.38
```

The `traceroute` tool can also be used to trace the route to a destination more accurately, which uses the TCP timeout method to determine when the route has been fully traced.

We send a TCP SYN packet to the destination device with a TTL of 1 in the IP header.

When the TCP SYN packet with a TTL greater than 1 reaches a router, the value of the TTL is decreased by 1, and the packet is forwarded to the next device. If the TCP SYN packet with a TTL of 1 reaches a router, the packet is dropped, and the router sends an ICMP Time-Exceeded packet back to us.

We receive the ICMP Time-Exceeded packet and note the IP address of the router that sent the packet.

After that, we send another TCP SYN packet to the destination, increasing the TTL by 1.

The process repeats until the TCP SYN packet reaches the destination host and receives a TCP SYN/ACK or a TCP RST response from the target. Once we receive a response from the destination device, we know that we have traced the route to the destination and ended the traceroute process.

### IP Payload

The payload (also referred to as IP Data) is the actual payload of the packet. It contains the data from various protocols, such as TCP or UDP, that are being transmitted, just like the contents of the letter in the envelope.

## TCP

TCP packets, also known as segments, are divided into several sections called headers and payloads. The TCP segments are wrapped in the sent IP packet.

The header contains several fields that contain important information. The source port indicates the computer from which the packet was sent. The destination port indicates to which computer the packet is sent. The sequence number indicates the order in which the data was sent. The confirmation number is used to confirm that all data was received successfully. The control flags indicate whether the packet marks the end of a message, whether it is an acknowledgment that data has been received, or whether it contains a request to repeat data. The window size indicates how much data the receiver can receive. The checksum is used to detect errors in the header and payload. The Urgent Pointer alerts the receiver that important data is in the payload.

The payload is the actual payload of the packet and contains the data that is being transmitted, just like the content of a conversation between two people.

## UDP

UDP transfers datagrams (small data packets) between two hosts. It is a connectionless protocol, meaning it does not need to establish a connection between the sender and the receiver before sending data. Instead, the data is sent directly to the target host without any prior connection.

When traceroute is used with UDP, we will receive a Destination Unreachable and Port Unreachable message when the UDP datagram packet reaches the target device. Generally, UDP packets are sent using traceroute on Unix hosts.

## Blind Spoofing

Blind spoofing, is a method of data manipulation attack in which an attacker sends false information on a network without seeing the actual responses sent back by the target devices. It involves manipulating the IP header field to indicate false source and destination addresses. For example, we send a TCP packet to the target host with false source and destination port numbers and a false Initial Sequence Number (ISN). The ISN is a field in the TCP header that is used to specify the sequence number of the first TCP packet in a connection. The ISN is set by the sender of a TCP packet and sent to the receiver in the TCP header of the first packet. This can cause the target host to establish a connection with us without receiving the connection.

This attack is commonly used to disrupt the integrity of network connections or to break connections between network devices. It can also be used to monitor network traffic or to intercept information sent by network devices.
