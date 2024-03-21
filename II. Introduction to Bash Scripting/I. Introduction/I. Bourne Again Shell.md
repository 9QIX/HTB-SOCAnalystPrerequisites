# Bourne Again Shell 🐚

Bash is the scripting language we use to communicate with Unix-based OS and give commands to the system. Since May 2019, Windows provides a Windows Subsystem for Linux that allows us to use Bash in a Windows environment. It is essential to master the language to work efficiently with it. The main difference between scripting and programming languages is that we don't need to compile the code to execute the scripting language, as opposed to programming languages.

As penetration testers, we must be able to work with any operating system, whether it is Windows or Unix-based. Efficiency depends mainly on the knowledge of the systems, especially in the privilege escalation field. On Unix-based systems, it is essential to learn how to use the terminal, filter data, and automate these processes. Especially in large Unix-based enterprise networks, we will have to deal with large amounts of data. We have to sort and filter out accordingly to determine potential gaps and information as fast as possible.

It is also essential to learn how to combine several commands and work with individual results. This is where scripting comes in, increasing our speed and efficiency. Like a programming language, a scripting language has almost the same structure, which can be divided into:

- Input & Output
- Arguments, Variables & Arrays
- Conditional execution
- Arithmetic
- Loops
- Comparison operators
- Functions

It is often common to automate some processes not to repeat them all the time or process and filter a large amount of information. In general, a script does not create a process, but it is executed by the interpreter that executes the script, in this case, the Bash. To execute a script, we have to specify the interpreter and tell it which script it should process. Such a call looks like this:

### Script Execution - Examples

```bash
# Bourne Again Shell
z0x9n@htb[/htb]$ bash script.sh <optional arguments>
# Bourne Again Shell
z0x9n@htb[/htb]$ sh script.sh <optional arguments>
# Bourne Again Shell
z0x9n@htb[/htb]$ ./script.sh <optional arguments>
```

Let us look at such a script and see how they can be created to get specific results. If we execute this script and specify a domain, we see what information this script provides.

### CIDR.sh

```bash
# Bourne Again Shell
z0x9n@htb[/htb]$ ./CIDR.sh inlanefreight.com


Discovered IP address(es):
165.22.119.202

Additional options available:

1. Identify the corresponding network range of the target domain.
2. Ping discovered hosts.
3. All checks.
   \*) Exit.

Select your option: 3

NetRange for 165.22.119.202:
NetRange: 165.22.0.0 - 165.22.255.255
CIDR: 165.22.0.0/16

Pinging host(s):
165.22.119.202 is up.

1 out of 1 hosts are up.
```

Now let us look at that script in detail and read it line by line in the best possible way. In the next sections, we will look at and analyze all the parts of this script.

### CIDR.sh

```bash
#!/bin/bash

# Check for given arguments
if [ $# -eq 0 ]; then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi

# Identify Network range for the specified IP address(es)
function network_range {
	for ip in $ipaddr; do
		netrange=$(whois $ip | grep "NetRange\|CIDR" | tee -a CIDR.txt)
		cidr=$(whois $ip | grep "CIDR" | awk '{print $2}')
		cidr_ips=$(prips $cidr)
		echo -e "\nNetRange for $ip:"
		echo -e "$netrange"
	done
}

# Ping discovered IP address(es)
function ping_host {
	hosts_up=0
	hosts_total=0

	echo -e "\nPinging host(s):"
	for host in $cidr_ips; do
		stat=1
		while [ $stat -eq 1 ]; do
			ping -c 2 $host > /dev/null 2>&1
			if [ $? -eq 0 ]; then
				echo "$host is up."
				((stat--))
				((hosts_up++))
				((hosts_total++))
			else
				echo "$host is down."
				((stat--))
				((hosts_total++))
			fi
		done
	done

	echo -e "\n$hosts_up out of $hosts_total hosts are up."
}

# Identify IP address of the specified domain
hosts=$(host $domain | grep "has address" | cut -d" " -f4 | tee discovered_hosts.txt)

echo -e "Discovered IP address:\n$hosts\n"
ipaddr=$(host $domain | grep "has address" | cut -d" " -f4 | tr "\n" " ")

# Available options
echo -e "Additional options available:"
echo -e "\t1) Identify the corresponding network range of the target domain."
echo -e "\t2) Ping discovered hosts."
echo -e "\t3) All checks."
echo -e "\t*) Exit.\n"

read -p "Select your option: " opt

case $opt in
	"1") network_range ;;
	"2") ping_host ;;
	"3") network_range && ping_host ;;
	"*") exit 0 ;;
esac
```

As we can see, we have commented here several parts of the script into which we can split it.

1. Check for given arguments
2. Identify network range for the specified IP address(es)
3. Ping discovered IP address(es)
4. Identify IP address(es) of the specified domain
5. Available options

In the first part of the script, we have an if-else statement that checks if we have specified a domain representing the target company.
