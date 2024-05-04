# Firewall and IDS/IPS Evasion

Nmap gives us many different ways to bypass firewalls rules and IDS/IPS. These methods include the fragmentation of packets, the use of decoys, and others that we will discuss in this section.

## Firewalls

A firewall is a security measure against unauthorized connection attempts from external networks. Every firewall security system is based on a software component that monitors network traffic between the firewall and incoming data connections and decides how to handle the connection based on the rules that have been set. It checks whether individual network packets are being passed, ignored, or blocked. This mechanism is designed to prevent unwanted connections that could be potentially dangerous.

## IDS/IPS

Like the firewall, the intrusion detection system (IDS) and intrusion prevention system (IPS) are also software-based components. IDS scans the network for potential attacks, analyzes them, and reports any detected attacks. IPS complements IDS by taking specific defensive measures if a potential attack should have been detected. The analysis of such attacks is based on pattern matching and signatures. If specific patterns are detected, such as a service detection scan, IPS may prevent the pending connection attempts.

## Determine Firewalls and Their Rules

We already know that when a port is shown as filtered, it can have several reasons. In most cases, firewalls have certain rules set to handle specific connections. The packets can either be dropped, or rejected. The dropped packets are ignored, and no response is returned from the host.

This is different for rejected packets that are returned with an RST flag. These packets contain different types of ICMP error codes or contain nothing at all.

Such errors can be:

- Net Unreachable
- Net Prohibited
- Host Unreachable
- Host Prohibited
- Port Unreachable
- Proto Unreachable

Nmap's TCP ACK scan (-sA) method is much harder to filter for firewalls and IDS/IPS systems than regular SYN (-sS) or Connect scans (sT) because they only send a TCP packet with only the ACK flag. When a port is closed or open, the host must respond with an RST flag. Unlike outgoing connections, all connection attempts (with the SYN flag) from external networks are usually blocked by firewalls. However, the packets with the ACK flag are often passed by the firewall because the firewall cannot determine whether the connection was first established from the external network or the internal network.

If we look at these scans, we will see how the results differ.

### SYN-Scan

```
z0x9n@htb[/htb]$ sudo nmap 10.129.2.28 -p 21,22,25 -sS -Pn -n --disable-arp-ping --packet-trace

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 14:56 CEST
SENT (0.0278s) TCP 10.10.14.2:57347 > 10.129.2.28:22 S ttl=53 id=22412 iplen=44  seq=4092255222 win=1024 <mss 1460>
SENT (0.0278s) TCP 10.10.14.2:57347 > 10.129.2.28:25 S ttl=50 id=62291 iplen=44  seq=4092255222 win=1024 <mss 1460>
SENT (0.0278s) TCP 10.10.14.2:57347 > 10.129.2.28:21 S ttl=58 id=38696 iplen=44  seq=4092255222 win=1024 <mss 1460>
RCVD (0.0329s) ICMP [10.129.2.28 > 10.10.14.2 Port 21 unreachable (type=3/code=3) ] IP [ttl=64 id=40884 iplen=72 ]
RCVD (0.0341s) TCP 10.129.2.28:22 > 10.10.14.2:57347 SA ttl=64 id=0 iplen=44  seq=1153454414 win=64240 <mss 1460>
RCVD (1.0386s) TCP 10.129.2.28:22 > 10.10.14.2:57347 SA ttl=64 id=0 iplen=44  seq=1153454414 win=64240 <mss 1460>
SENT (1.1366s) TCP 10.10.14.2:57348 > 10.129.2.28:25 S ttl=44 id=6796 iplen=44  seq=4092320759 win=1024 <mss 1460>
Nmap scan report for 10.129.2.28
Host is up (0.0053s latency).

PORT   STATE    SERVICE
21/tcp filtered ftp
22/tcp open     ssh
25/tcp filtered smtp
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
```

### ACK-Scan

```
z0x9n@htb[/htb]$ sudo nmap 10.129.2.28 -p 21,22,25 -sA -Pn -n --disable-arp-ping --packet-trace

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-21 14:57 CEST
SENT (0.0422s) TCP 10.10.14.2:49343 > 10.129.2.28:21 A ttl=49 id=12381 iplen=40  seq=0 win=1024
SENT (0.0423s) TCP 10.10.14.2:49343 > 10.129.2.28:22 A ttl=41 id=5146 iplen=40  seq=0 win=1024
SENT (0.0423s) TCP 10.10.14.2:49343 > 10.129.2.28:25 A ttl=49 id=5800 iplen=40  seq=0 win=1024
RCVD (0.1252s) ICMP [10.129.2.28 > 10.10.14.2 Port 21 unreachable (type=3/code=3) ] IP [ttl=64 id=55628 iplen=68 ]
RCVD (0.1268s) TCP 10.129.2.28:22 > 10.10.14.2:49343 R ttl=64 id=0 iplen=40  seq=1660784500 win=0
SENT (1.3837s) TCP 10.10.14.2:49344 > 10.129.2.28:25 A ttl=59 id=21915 iplen=40  seq=0 win=1024
Nmap scan report for 10.129.2.28
Host is up (0.083s latency).

PORT   STATE      SERVICE
21/tcp filtered   ftp
22/tcp unfiltered ssh
25/tcp filtered   smtp
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 0.15 seconds
```

| Scanning Options     | Description                           |
| -------------------- | ------------------------------------- |
| `10.129.2.28`        | Scans the specified target.           |
| `-p 21,22,25`        | Scans only the specified ports.       |
| `-sS`                | Performs SYN scan on specified ports. |
| `-sA`                | Performs ACK scan on specified ports. |
| `-Pn`                | Disables ICMP Echo requests.          |
| `-n`                 | Disables DNS resolution.              |
| `--disable-arp-ping` | Disables ARP ping.                    |
| `--packet-trace`     | Shows all packets sent and received.  |

Please pay attention to the RCVD packets and its set flag we receive from our target. With the SYN scan (-sS) our target tries to establish the TCP connection by sending a packet back with
