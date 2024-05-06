# Firewall and IDS/IPS Evasion - Hard Lab

With our second test's help, our client was able to gain new insights and sent one of its administrators to a training course for IDS/IPS systems. As our client told us, the training would last one week. Now the administrator has taken all the necessary precautions and wants us to test this again because specific services must be changed, and the communication for the provided software had to be modified.

## Question

Now our client wants to know if it is possible to find out the version of the running services. Identify the version of service our client was talking about and submit the flag as the answer.

```bash
sudo nmap -p- -Pn -g 53 <ip>
```

- `sudo`: This command is run with superuser privileges.
- `nmap`: This invokes the Nmap tool, which is used for network discovery and security auditing.
- `-p-`: This option tells Nmap to scan all ports. The hyphen (`-`) specifies a port range from 1 to 65535.
- `-Pn`: This option tells Nmap not to perform host discovery. Without this option, Nmap would attempt to determine whether the host is up before scanning for open ports.
- `-g 53`: This option sets the source port to 53 (DNS). This can be useful for bypassing certain firewall restrictions or for simulating DNS traffic.
- `<ip>`: This is the IP address of the target host.

Explanation: This command performs a comprehensive port scan on the target host, scanning all 65,535 TCP ports. It does not perform host discovery and sets the source port to 53, which may help in bypassing firewall restrictions. However, using port 53 as the source port may also trigger intrusion detection systems (IDS) or intrusion prevention systems (IPS) since port 53 is commonly associated with DNS traffic.

```bash
sudo nc -nv -p <ip> 50000
```

- `sudo`: This command is run with superuser privileges.
- `nc`: This invokes the netcat tool, which is used for reading from and writing to network connections.
- `-nv`: These options make netcat operate in verbose mode, displaying more information about the connection.
- `-p 53`: This option sets the source port to 53 (DNS).
- `10.129.2.47`: This is the IP address of the target host.
- `50000`: This is the destination port to connect to on the target host.

Explanation: This command establishes a TCP connection to the target host (`10.129.2.47`) on port `50000`, using port `53` as the source port. The purpose of this command is not entirely clear without context, but it could be used to test network connectivity or to communicate with a service running on port `50000` of the target host. However, using port `53` as the source port may trigger IDS/IPS systems, as port `53` is typically associated with DNS traffic.
