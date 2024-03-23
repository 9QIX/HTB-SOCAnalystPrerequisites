# Comparison Operators 📊

To compare specific values with each other, we need elements that are called comparison operators. The comparison operators are used to determine how the defined values will be compared. For these operators, we differentiate between:

- string operators
- integer operators
- file operators
- boolean operators

## String Operators

When comparing strings, specific operators determine how the defined values will be compared:

| Operator | Description                                 |
| -------- | ------------------------------------------- |
| ==       | is equal to                                 |
| !=       | is not equal to                             |
| <        | is less than in ASCII alphabetical order    |
| >        | is greater than in ASCII alphabetical order |
| -z       | if the string is empty (null)               |
| -n       | if the string is not null                   |

```bash
#!/bin/bash

# Check the given argument
if [ "$1" != "HackTheBox" ]; then
    echo -e "You need to give 'HackTheBox' as an argument."
    exit 1
elif [ $# -gt 1 ]; then
    echo -e "Too many arguments given."
    exit 1
else
    domain=$1
    echo -e "Success!"
fi
```

String comparison operators "< / >" work only within the double square brackets `[[ <condition> ]]`.

```bash
z0x9n@htb[/htb]$ man ascii
```

| Decimal | Hexadecimal | Character | Description     |
| ------- | ----------- | --------- | --------------- |
| 0       | 00          | NUL       | End of a string |
| ...     | ...         | ...       | ...             |
| 65      | 41          | A         | Capital A       |
| 66      | 42          | B         | Capital B       |
| 67      | 43          | C         | Capital C       |
| 68      | 44          | D         | Capital D       |
| ...     | ...         | ...       | ...             |
| 127     | 7F          | DEL       | Delete          |

ASCII stands for American Standard Code for Information Interchange and represents a 7-bit character encoding. Since each bit can take two values, there are 128 different bit patterns, which can also be interpreted as the decimal integers 0 - 127 or in hexadecimal values 00 - 7F. The first 32 ASCII character codes are reserved as so-called control characters.

## Integer Operators

Integer comparison operators help in comparing integer numbers:

| Operator | Description                 |
| -------- | --------------------------- |
| -eq      | is equal to                 |
| -ne      | is not equal to             |
| -lt      | is less than                |
| -le      | is less than or equal to    |
| -gt      | is greater than             |
| -ge      | is greater than or equal to |

```bash
#!/bin/bash

# Check the given argument
if [ $# -lt 1 ]; then
    echo -e "Number of given arguments is less than 1"
    exit 1
elif [ $# -gt 1 ]; then
    echo -e "Number of given arguments is greater than 1"
    exit 1
else
    domain=$1
    echo -e "Number of given arguments equals 1"
fi
```

## File Operators

File operators are useful for checking specific file conditions:

| Operator | Description                                 |
| -------- | ------------------------------------------- |
| -e       | if the file exists                          |
| -f       | tests if it is a file                       |
| -d       | tests if it is a directory                  |
| -L       | tests if it is if a symbolic link           |
| -N       | checks if the file was modified             |
| -O       | if the current user owns the file           |
| -G       | if the file’s group id matches the user’s   |
| -s       | tests if the file has a size greater than 0 |
| -r       | tests if the file has read permission       |
| -w       | tests if the file has write permission      |
| -x       | tests if the file has execute permission    |

```bash
#!/bin/bash

# Check if the specified file exists
if [ -e "$1" ]; then
    echo -e "The file exists."
    exit 0
else
    echo -e "The file does not exist."
    exit 2
fi
```

## Boolean and Logical Operators

Boolean and logical operators help in obtaining boolean values:

```bash
#!/bin/bash

# Check the boolean value
if [[ -z $1 ]]; then
    echo -e "Boolean value: True (is null)"
    exit 1
elif [[ $# > 1 ]]; then
    echo -e "Boolean value: True (is greater than)"
    exit 1
else
    domain=$1
    echo -e "Boolean value: False (is equal to)"
fi
```

Logical operators help in defining multiple conditions:

| Operator | Description          |
| -------- | -------------------- |
| !        | logical negation NOT |
| &&       | logical AND          |
| \|\|     | logical OR           |

```bash
#!/bin/bash

# Check if the specified file exists and if we have read permissions
if [[ -e "$1" && -r "$1" ]]; then
    echo -e "We can read the specified file."
    exit 0
elif [[ ! -e "$1" ]]; then
    echo -e "The specified file does not exist."
    exit 2
elif [[ -e "$1" && ! -r "$1" ]]; then
    echo -e "We don't have read permission for this file."
    exit 1
else
    echo -e "An error occurred."
    exit 5
fi
```
