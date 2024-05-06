# **Firewall and IDS/IPS Evasion - Medium Lab**

After we conducted the first test and submitted our results to our client, the administrators made some changes and improvements to the `IDS/IPS` and firewall. We could hear that the administrators were not satisfied with their previous configurations during the meeting, and they could see that the network traffic could be filtered more strictly.

## Question

After the configurations are transferred to the system, our client wants to know if it is possible to find out our target's DNS server version. Submit the DNS server version of the target as the answer.

sudo nmap -sS -sU -p 53 --script dns-nsid --max-retries 0 --min-rate 1000 --max-parallelism 5 -T2 -n 10.129.2.48
