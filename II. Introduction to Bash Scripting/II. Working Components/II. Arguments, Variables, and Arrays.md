# Arguments, Variables, and Arrays 📝

## Arguments

The advantage of bash scripts is that we can always pass up to 9 arguments ($0-$9) to the script without assigning them to variables or setting the corresponding requirements for these. 9 arguments because the first argument $0 is reserved for the script. As we can see here, we need the dollar sign ($) before the name of the variable to use it at the specified position. The assignment would look like this in comparison:

```bash
z0x9n@htb[/htb]$ ./script.sh ARG1 ARG2 ARG3 ... ARG9
    ASSIGNMENTS: $0 $1 $2 $3 ... $9
```

This means that we have automatically assigned the corresponding arguments to the predefined variables in this place. These variables are called special variables. These special variables serve as placeholders. If we now look at the code section again, we will see where and which arguments have been used.

### CIDR.sh

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

There are several ways how we can execute our script. However, we must first set the script's execution privileges before executing it with the interpreter defined in it.

#### CIDR.sh - Set Execution Privileges

```bash
z0x9n@htb[/htb]$ chmod +x cidr.sh
```

#### CIDR.sh - Execution without Arguments

```bash
z0x9n@htb[/htb]$ ./cidr.sh

You need to specify the target domain.

Usage:
	cidr.sh <domain>
```

#### CIDR.sh - Execution without Execution Permissions

```bash
z0x9n@htb[/htb]$ bash cidr.sh

You need to specify the target domain.

Usage:
	cidr.sh domain
```

## Special Variables

Special variables use the Internal Field Separator (IFS) to identify when an argument ends and the next begins. Bash provides various special variables that assist while scripting. Some of these variables are:

| IFS | Description                                                                                                                                                             |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $#  | This variable holds the number of arguments passed to the script.                                                                                                       |
| $@  | This variable can be used to retrieve the list of command-line arguments.                                                                                               |
| $n  | Each command-line argument can be selectively retrieved using its position. For example, the first argument is found at $1.                                             |
| $$  | The process ID of the currently executing process.                                                                                                                      |
| $?  | The exit status of the script. This variable is useful to determine a command's success. The value 0 represents successful execution, while 1 is a result of a failure. |

Of the ones shown above, we have 3 such special variables in our if-else condition.

| Variable | Description                                                                                                                                                                                                                                     |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $#       | In this case, we need just one variable that needs to be assigned to the domain variable. This variable is used to specify the target we want to work with. If we provide just an FQDN as the argument, the $# variable will have a value of 1. |
| $0       | This special variable is assigned the name of the executed script, which is then shown in the "Usage:" example.                                                                                                                                 |
| $1       | Separated by a space, the first argument is assigned to that special variable.                                                                                                                                                                  |

This table provides a summary of special variables typically used in shell scripts and their descriptions.

## Variables

We also see at the end of the if-else loop that we assign the value of the first argument to the variable called "domain". The assignment of variables takes place without the dollar sign ($). The dollar sign is only intended to allow this variable's corresponding value to be used in other code sections. When assigning variables, there must be no spaces between the names and values.

```bash
else
	domain=$1
fi
```

In contrast to other programming languages, there is no direct differentiation and recognition between the types of variables in Bash like "strings," "integers," and "boolean." All contents of the variables are treated as string characters. Bash enables arithmetic functions depending on whether only numbers are assigned or not. It is important to note when declaring variables that they do not contain a space. Otherwise, the actual variable name will be interpreted as an internal function or a command.

#### Declaring a Variable - Error

```bash
z0x9n@htb[/htb]$ variable = "this will result with an error."

command not found: variable
```

#### Declaring a Variable - Without an Error

```bash
z0x9n@htb[/htb]$ variable="Declared without an error."
z0x9n@htb[/htb]$ echo $variable

Declared without an error.
```

## Arrays

There is also the possibility of assigning several values to a single variable in Bash. This can be beneficial if we want to scan multiple domains or IP addresses. These variables are called arrays that we can use to store and process an ordered sequence of specific type values. Arrays identify each stored entry with an index starting with 0. When we want to assign a value to an array component, we do so in the same way as with standard shell variables. All we do is specify the field index enclosed in square brackets. The declaration for arrays looks like this in Bash:

### Arrays.sh

```bash
#!/bin/bash

domains=(www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com www2.inlanefreight.com)

echo ${domains[0]}
```

We can also retrieve them individually using the index using the variable with the corresponding index in curly brackets. Curly brackets are used for variable expansion.

```bash
z0x9n@htb[/htb]$ ./Arrays.sh

www.inlanefreight.com
```

It is important to note that single quotes (' ... ') and double quotes (" ... ") prevent the separation by a space of the individual values in the array. This means that all spaces between the single and double quotes are

### Arrays.sh

```bash
#!/bin/bash

domains=("www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com" www2.inlanefreight.com)
echo ${domains[0]}
```

```bash
z0x9n@htb[/htb]$ ./Arrays.sh

www.inlanefreight.com ftp.inlanefreight.com vpn.inlanefreight.com
```
