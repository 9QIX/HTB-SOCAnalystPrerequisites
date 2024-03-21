# Conditional Execution 🚦

Conditional execution allows us to control the flow of our script by reaching different conditions. This function is one of the essential components. Otherwise, we could only execute one command after another.

When defining various conditions, we specify which functions or sections of code should be executed for a specific value. If we reach a specific condition, only the code for that condition is executed, and the others are skipped. As soon as the code section is completed, the following commands will be executed outside the conditional execution. Let us look at the first part of the script again and analyze it.

### Script.sh

```bash
#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]; then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
else
	domain=$1
fi
```

In summary, this code section works with the following components:

- `#!/bin/bash` - Shebang.
- `if-else-fi` - Conditional execution.
- `echo` - Prints specific output.
- `$#` / `$0` / `$1` - Special variables.
- `domain` - Variables.

The conditions of the conditional executions can be defined using variables (`$#`, `$0`, `$1`, `domain`), values (`0`), and strings, as we will see in the next examples. These values are compared with the comparison operators (`-eq`) that we will look at in the next section.

### Shebang

The shebang line is always at the top of each script and always starts with `#!`. This line contains the path to the specified interpreter (`/bin/bash`) with which the script is executed. We can also use Shebang to define other interpreters like Python, Perl, and others.

```python
#!/usr/bin/env python
```

```perl
#!/usr/bin/env perl
```

### If-Else-Fi

One of the most fundamental programming tasks is to check different conditions to deal with these. Checking of conditions usually has two different forms in programming and scripting languages, the if-else condition and case statements. In pseudo-code, the if condition means the following:

#### Pseudo-Code

```bash
if [ the number of given arguments equals 0 ]; then
	Print: "You need to specify the target domain."
	Print: "<empty line>"
	Print: "Usage:"
	Print: "   <name of the script> <domain>"
	Exit the script with an error
else
	The "domain" variable serves as the alias for the given argument
finish the if-condition
```

By default, an If-Else condition can contain only a single "If", as shown in the next example.

### If-Only.sh

```bash
#!/bin/bash

value=$1

if [ $value -gt "10" ]; then
        echo "Given argument is greater than 10."
fi
```

#### If-Only.sh - Execution

```bash
  # Conditional Execution
z0x9n@htb[/htb]$ bash if-only.sh 5
  # Conditional Execution
z0x9n@htb[/htb]$ bash if-only.sh 12

Given argument is greater than 10.
```

When adding Elif or Else, we add alternatives to treat specific values or statuses. If a particular value does not apply to the first case, it will be caught by others.

### If-Elif-Else.sh

```bash
#!/bin/bash

value=$1

if [ $value -gt "10" ]; then
	echo "Given argument is greater than 10."
elif [ $value -lt "10" ]; then
	echo "Given argument is less than 10."
else
	echo "Given argument is not a number."
fi
```

#### If-Elif-Else.sh - Execution

```bash
  # Conditional Execution
z0x9n@htb[/htb]$ bash if-elif-else.sh 5

Given argument is less than 10.

  # Conditional Execution
z0x9n@htb[/htb]$ bash if-elif-else.sh 12

Given argument is greater than 10.

  # Conditional Execution
z0x9n@htb[/htb]$ bash if-elif-else.sh HTB

if-elif-else.sh: line 5: [: HTB: integer expression expected
if-elif-else.sh: line 8: [: HTB: integer expression expected
Given argument is not a number.
```

We could extend our script and specify several conditions. This could look something like this:

### Several Conditions - Script.sh

```bash
#!/bin/bash

# Check for given argument
if [ $# -eq 0 ]; then
	echo -e "You need to specify the target domain.\n"
	echo -e "Usage:"
	echo -e "\t$0 <domain>"
	exit 1
elif [ $# -eq 1 ]; then
	domain=$1
else
	echo -e "Too many arguments given."
	exit 1
fi
```

Here we define another condition (elif [<condition>];then) that prints a line telling us (echo -e "...") that we have given more than one argument and exits the program with an error (exit 1).

### Exercise Script

Create an "If-Else" condition in the "For"-Loop of the "Exercise Script" that prints you the number of characters of the 35th generated value of the variable "var". Submit the number as the answer.

```bash
#!/bin/bash
# Count number of characters in a variable:
#     echo $variable | wc -c

# Variable to encode
var="nef892na9s1p9asn2aJs71nIsm"

for counter in {1..40}
do
        var=$(echo $var | base64)
done
```

### Asnwer
