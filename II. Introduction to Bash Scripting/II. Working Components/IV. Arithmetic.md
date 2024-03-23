# Arithmetic

In Bash, we have seven different arithmetic operators we can work with. These are used to perform different mathematical operations or to modify certain integers.

#### Arithmetic Operators

| Operator   | Description                             |
| ---------- | --------------------------------------- |
| +          | Addition                                |
| -          | Subtraction                             |
| \*         | Multiplication                          |
| /          | Division                                |
| %          | Modulus                                 |
| variable++ | Increase the value of the variable by 1 |
| variable-- | Decrease the value of the variable by 1 |

We can summarize all these operators in a small script:

```bash
#!/bin/bash

increase=1
decrease=1

echo "Addition: 10 + 10 = $((10 + 10))"
echo "Subtraction: 10 - 10 = $((10 - 10))"
echo "Multiplication: 10 * 10 = $((10 * 10))"
echo "Division: 10 / 10 = $((10 / 10))"
echo "Modulus: 10 % 4 = $((10 % 4))"

((increase++))
echo "Increase Variable: $increase"

((decrease--))
echo "Decrease Variable: $decrease"
```

The output of this script looks like this:

#### Arithmetic.sh - Execution

```bash
z0x9n@htb[/htb]$ ./Arithmetic.sh

Addition: 10 + 10 = 20
Subtraction: 10 - 10 = 0
Multiplication: 10 * 10 = 100
Division: 10 / 10 = 1
Modulus: 10 % 4 = 2
Increase Variable: 2
Decrease Variable: 0
```

We can also calculate the length of the variable. Using this function `${#variable}`, every character gets counted, and we get the total number of characters in the variable.

```bash
#!/bin/bash

htb="HackTheBox"

echo ${#htb}
```

If we look at our `CIDR.sh` script, we will see that we have used the increase and decrease operators several times. This ensures that the while loop, which we will discuss later, runs and pings the hosts while the variable "stat" has a value of 1. If the ping command ends with code 0 (successful), we get a message that the host is up and the "stat" variable, as well as the variables "hosts_up" and "hosts_total" get changed.

```bash
<SNIP>
	echo -e "\nPinging host(s):"
	for host in $cidr_ips
	do
		stat=1
		while [ $stat -eq 1 ]
		do
			ping -c 2 $host > /dev/null 2>&1
			if [ $? -eq 0 ]
			then
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
<SNIP>
```
