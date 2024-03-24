# Debugging

Debugging in Bash involves identifying and resolving errors (bugs) in our code. It's a crucial process for ensuring script functionality and reliability. Bash provides several debugging options, such as the `-x` (xtrace) and `-v` options, which help track the execution of commands and functions.

#### Using `-x` Option

The `-x` option traces the execution of each command, showing exactly which commands are being executed and with what values. Here's an example using our `CIDR.sh` script:

```bash
z0x9n@htb[/htb]$ bash -x CIDR.sh

+ '[' 0 -eq 0 ']'
+ echo -e 'You need to specify the target domain.\n'
You need to specify the target domain.

+ echo -e Usage:
Usage:
+ echo -e '\tCIDR.sh <domain>'
	CIDR.sh <domain>
+ exit 1
```

In this output, each line preceded by a `+` sign indicates a command being executed.

#### Using `-x` and `-v` Options Together

Combining the `-x` and `-v` options provides more detailed output, including the script's entire code for each executed command. Here's how to use them:

```bash
z0x9n@htb[/htb]$ bash -x -v CIDR.sh

#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]
then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi
+ '[' 0 -eq 0 ']'
+ echo -e 'You need to specify the target domain.\n'
You need to specify the target domain.

+ echo -e Usage:
Usage:
+ echo -e '\tCIDR.sh <domain>'
	CIDR.sh <domain>
+ exit 1
```

In this output, the entire script code is displayed along with the execution trace for each command.

These debugging options are invaluable for identifying and fixing errors in Bash scripts, enhancing script reliability and functionality.
