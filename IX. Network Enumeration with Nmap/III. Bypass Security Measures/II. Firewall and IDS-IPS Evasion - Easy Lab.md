# **Firewall and IDS/IPS Evasion - Easy Lab**

Now let's get practical. A company hired us to test their IT security defenses, including their `IDS` and `IPS` systems. Our client wants to increase their IT security and will, therefore, make specific improvements to their `IDS/IPS` systems after each successful test. We do not know, however, according to which guidelines these changes will be made. Our goal is to find out specific information from the given situations.

We are only ever provided with a machine protected by `IDS/IPS` systems and can be tested. For learning purposes and to get a feel for how `IDS/IPS` can behave, we have access to a status web page at:

This page shows us the number of alerts. We know that if we receive a specific amount of alerts, we will be `banned`. Therefore we have to test the target system as `quietly` as possible.

## Questions

Our client wants to know if we can identify which operating system their provided machine is running on. Submit the OS name as the answer.

```bash
sudo nmap -sS -sV -Pn -n --disable-arp-ping --packet-trace -p 80 --reason -T4 <ip>
```

Explanation of options used:

- `-sS`: Performs a SYN scan, which is a stealthy scan method.
- `-Pn`: Treats all hosts as online, skipping the host discovery phase.
- `-n`: Disables DNS resolution to avoid sending DNS queries.
- `--disable-arp-ping`: Disables ARP ping to avoid network discovery.
- `--packet-trace`: Displays all sent and received packets for debugging purposes.
- `-p 80`: Specifies port 80 (HTTP) to scan, as the target webpage is likely hosted on this port.
- `--reason`: Shows the reason for the scan results.
- `-T4`: Sets the timing template to aggressive to speed up the scan.

This command will perform a SYN scan on port 80 of the target IP address, aiming to evade IDS/IPS detection while gathering information about the web server running on the target.
