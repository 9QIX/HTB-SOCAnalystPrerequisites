# Host and Port Scanning

It is essential to understand how the tool we use works and how it performs and processes the different functions. We will only understand the results if we know what they mean and how they are obtained. Therefore we will take a closer look at and analyze some of the scanning methods. After we have found out that our target is alive, we want to get a more accurate picture of the system. The information we need includes:

- Open ports and its services
- Service versions
- Information that the services provided
- Operating system

There are a total of 6 different states for a scanned port we can obtain:

| State            | Description                                                                                                                                                                                           |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| open             | This indicates that the connection to the scanned port has been established. These connections can be TCP connections, UDP datagrams as well as SCTP associations.                                    |
| closed           | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an RST flag. This scanning method can also be used to determine if our target is alive or not. |
| filtered         | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.                |
| unfiltered       | This state of a port only occurs during the TCP-ACK scan and means that the port is accessible, but it cannot be determined whether it is open or closed.                                             |
| open\|filtered   | If we do not get a response for a specific port, Nmap will set it to that state. This indicates that a firewall or packet filter may protect the port.                                                |
| closed\|filtered | This state only occurs in the IP ID idle scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.                                             |

## Discovering Open TCP Ports

By default, Nmap scans the top 1000 TCP ports with the SYN scan (`-sS`). This SYN scan is set only to default when we run it as root because of the socket permissions required to create raw TCP packets. Otherwise, the TCP scan (`-sT`) is performed by default. This means that if we do not define ports and scanning methods, these parameters are set automatically. We can define the ports one by one (`-p 22,25,80,139,445`), by range (`-p 22-445`), by top ports (`--top-ports=10`) from the Nmap database that have been signed as most frequent, by scanning all ports (`-p-`) but also by defining a fast port scan, which contains top 100 ports (`-F`).

### Scanning Top 10 TCP Ports

```
z0x9n@htb[/htb]$ sudo nmap 10.129.2.28 --top-ports=10

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:36 CEST
Nmap scan report for 10.129.2.28
Host is up (0.021s latency).

PORT     STATE    SERVICE
21/tcp   closed   ftp
22/tcp   open     ssh
23/tcp   closed   telnet
25/tcp   open     smtp
80/tcp   open     http
110/tcp  open     pop3
139/tcp  filtered netbios-ssn
443/tcp  closed   https
445/tcp  filtered microsoft-ds
3389/tcp closed   ms-wbt-server
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 1.44 seconds
```

| Scanning Options | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| `10.129.2.28`    | Scans the specified target.                                            |
| `--top-ports=10` | Scans the specified top ports that have been defined as most frequent. |

We see that we only scanned the top 10 TCP ports of our target, and Nmap displays their state accordingly. If we trace the packets Nmap sends, we will see the RST flag on TCP port 21 that our target sends back to us. To have a clear view of the SYN scan, we disable the ICMP echo requests (`-Pn`), DNS resolution (`-n`), and ARP ping scan (`--disable-arp-ping`).

### Nmap - Trace the Packets

```
z0x9n@htb[/htb]$ sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 15:39 CEST
SENT (0.0429s) TCP 10.10.14.2:63090 > 10.129.2.28:21 S ttl=56 id=57322 iplen=44  seq=1699105818 win=1024 <mss 1460>
RCVD (0.0573s) TCP 10.129.2.28:21 > 10.10.14.2:63090 RA ttl=64 id=0 iplen=40  seq=0 win=0
Nmap scan report for 10.11.1.28
Host is up (0.014s latency).

PORT   STATE  SERVICE
21/tcp closed ftp
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
```

| Scanning Options     | Description                          |
| -------------------- | ------------------------------------ |
| `10.129.2.28`        | Scans the specified target.          |
| `-p 21`              | Scans only the specified port.       |
| `--packet-trace`     | Shows all packets sent and received. |
| `-n`                 | Disables DNS resolution.             |
| `--disable-arp-ping` | Disables ARP ping.                   |

We can see from the `SENT` line that we (`10.10.14.2`) sent a TCP packet with the SYN flag (`S`) to our target (`10.129.2.28`). In the next `RCVD` line, we can see that the target responds with a TCP packet containing the RST and ACK flags (`RA`). RST and ACK flags are used to acknowledge receipt of the TCP packet (ACK) and to end the TCP session (RST).

| Request                                                       | Description                                                                                      |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| `SENT (0.0429s)`                                              | Indicates the SENT operation of Nmap, which sends a packet to the target.                        |
| `TCP`                                                         | Shows the protocol that is being used to interact with the target port.                          |
| `10.10.14.2:63090 >`                                          | Represents our IPv4 address and the source port, which will be used by Nmap to send the packets. |
| `10.129.2.28:21`                                              | Shows the target IPv4 address and the target port.                                               |
| `S`                                                           | SYN flag of the sent TCP packet.                                                                 |
| `ttl=56 id=57322 iplen=44 seq=1699105818 win=1024 <mss 1460>` | Additional TCP Header parameters.                                                                |

| Response                           | Description                                                                       |
| ---------------------------------- | --------------------------------------------------------------------------------- |
| `RCVD (0.0573s)`                   | Indicates a received packet from the target.                                      |
| `TCP`                              | Shows the protocol that is being used.                                            |
| `10.129.2.28:21 >`                 | Represents targets IPv4 address and the source port, which will be used to reply. |
| `10.10.14.2:63090`                 | Shows our IPv4 address and the port that will be replied to.                      |
| `RA`                               | RST and ACK flags of the sent TCP packet.                                         |
| `ttl=64 id=0 iplen=40 seq=0 win=0` | Additional TCP Header parameters.                                                 |

### Connect Scan

The Nmap TCP Connect Scan (`-sT`) uses the TCP three-way handshake to determine if a specific port on a target host is open or closed. The scan sends an SYN packet to the target port and waits for a response. It is considered open if the target port responds with an SYN-ACK packet and closed if it responds with an RST packet.

The Connect scan is useful because it is the most accurate way to determine the state of a port, and it is also the most stealthy. Unlike other types of scans, such as the SYN scan, the Connect scan does not leave any unfinished connections or unsent packets on the target host, which makes it less likely to be detected by intrusion detection systems (IDS) or intrusion prevention systems (IPS). It is useful when we want to map the network and don't want to disturb the services running behind it,
