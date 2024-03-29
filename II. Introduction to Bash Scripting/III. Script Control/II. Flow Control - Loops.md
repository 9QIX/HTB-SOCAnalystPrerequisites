# Flow Control - Loops

#### For Loops

Let us start with the For loops. The For loop is executed on each pass for precisely one parameter, which the shell takes from a list, calculates from an increment, or takes from another data source. The for loop runs as long as it finds corresponding data. This type of loop can be structured and defined in different ways. For example, the for loops are often used when we need to work with many different values from an array. This can be used to scan different hosts or ports. We can also use it to execute specific commands for known ports and their services to speed up our enumeration process.

```bash
for variable in 1 2 3 4
do
	echo $variable
done
```

```bash
for variable in file1 file2 file3
do
	echo $variable
done
```

```bash
for ip in "10.10.10.170 10.10.10.174 10.10.10.175"
do
	ping -c 1 $ip
done
```

Of course, we can also write these commands in a single line.

```bash
z0x9n@htb[/htb]$ for ip in 10.10.10.170 10.10.10.174;do ping -c 1 $ip;done
```

Let us have another look at our CIDR.sh script. We have added several for loops to the script, but let us stick with this little code section.

```bash
<SNIP>

# Identify Network range for the specified IP address(es)
function network_range {
	for ip in $ipaddr
	do
		netrange=$(whois $ip | grep "NetRange\|CIDR" | tee -a CIDR.txt)
		cidr=$(whois $ip | grep "CIDR" | awk '{print $2}')
		cidr_ips=$(prips $cidr)
		echo -e "\nNetRange for $ip:"
		echo -e "$netrange"
	done
}

<SNIP>
```

As in the previous example, for each IP address from the array "ipaddr" we make a "whois" request, whose output is filtered for "NetRange" and "CIDR." This helps us to determine which address range our target is located in. We can use this information to search for additional hosts during a penetration test, if approved by the client. The results that we receive are displayed accordingly and stored in the file "CIDR.txt."

#### While Loops

The while loop is conceptually simple and follows the following principle:

A statement is executed as long as a condition is fulfilled (true).

```bash
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
```

The while loops also work with conditions like if-else. A while loop needs some sort of a counter to orientate itself when it has to stop executing the commands it contains. Otherwise, this leads to an endless loop. Such a counter can be a variable that we have declared with a specific value or a boolean value. While loops run while the boolean value is "True". Besides the counter, we can also use the command "break," which interrupts the loop when reaching this command like in the following example:

```bash
#!/bin/bash

counter=0

while [ $counter -lt 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"

  if [ $counter == 2 ]
  then
    continue
  elif [ $counter == 4 ]
  then
    break
  fi
done
```

```bash
z0x9n@htb[/htb]$ ./WhileBreaker.sh

Counter: 1
Counter: 2
Counter: 3
Counter: 4
```

#### Until Loops

There is also the until loop, which is relatively rare. Nevertheless, the until loop works precisely like the while loop, but with the difference:

The code inside a until loop is executed as long as the particular condition is false.

```bash
#!/bin/bash

counter=0

until [ $counter -eq 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"
done
```

```bash
z0x9n@htb[/htb]$ ./Until.sh

Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
Counter: 6
Counter: 7
Counter: 8
Counter: 9
Counter: 10
```

### Exercise Script

Create a "For" loop that encodes the variable "var" 28 times in "base64". The number of characters in the 28th hash is the value that must be assigned to the "salt" variable.

```bash
#!/bin/bash

# Decrypt function
function decrypt {
	MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
	Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
	MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')

	flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}

# Variables
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

# <- For-Loop here

# Check if $salt is empty
if [[ ! -z "$salt" ]]
then
	decrypt
	echo $flag
else
	exit 1
fi
```

### Answer

```bash
#!/bin/bash

# Decrypt function
function decrypt {
    # Perform multiple string substitutions to transform the hash
    MzSaas7k=$(echo $hash | sed 's/988sn1/83unasa/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4d298d/9999/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/3i8dqos82/873h4d/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/4n9Ls/20X/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/912oijs01/i7gg/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/k32jx0aa/n391s/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/nI72n/YzF1/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/82ns71n/2d49/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/JGcms1a/zIm12/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/MS9/4SIs/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/Ymxj00Ims/Uso18/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/sSi8Lm/Mit/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/9su2n/43n92ka/g')
    Mzns7293sk=$(echo $MzSaas7k | sed 's/ggf3iunds/dn3i8/g')
    MzSaas7k=$(echo $Mzns7293sk | sed 's/uBz/TT0K/g')

    # Decrypt the modified hash using base64 and openssl
    flag=$(echo $MzSaas7k | base64 -d | openssl enc -aes-128-cbc -a -d -salt -pass pass:$salt)
}

# Variables
var="9M"
salt=""
hash="VTJGc2RHVmtYMTl2ZnYyNTdUeERVRnBtQWVGNmFWWVUySG1wTXNmRi9rQT0K"

# Base64 Encoding Example:
#        $ echo "Some Text" | base64

# For loop to encode 'var' 28 times
for i in {1..28}
do
    var=$(echo "$var" | base64 )
done

# Assign the number of characters in the 28th hash to the "salt" variable
salt=$(( ${#var} + 1 ))

# Check if $salt is not empty
if [[ ! -z "$salt" ]]
then
    decrypt
    echo $flag
else
    exit 1
fi
```

```bash
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.

HTBL00p5r0x
```

Explanation:

- The script defines a function `decrypt` to perform decryption.
- Inside the function, multiple string substitutions are performed to transform the hash.
- The hash is decrypted using base64 and openssl in the `decrypt` function.
- Variables `var`, `salt`, and `hash` are initialized.
- A for loop is used to encode the variable `var` 28 times in base64.
- The number of characters in the 28th hash is assigned to the `salt` variable.
- Finally, the script checks if `salt` is not empty, then calls the `decrypt` function and prints the result. Otherwise, it exits with an error code.
