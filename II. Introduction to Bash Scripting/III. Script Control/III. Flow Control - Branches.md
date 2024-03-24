# Flow Control - Branches

As we have already seen, the branches in flow control include if-else and the case statements. We have already discussed the if-else statements in detail and know how this works. Now we will take a closer look at the case statements.

#### Case Statements

Case statements are also known as switch-case statements in other languages, such as C/C++ and C#. The main difference between if-else and switch-case is that if-else constructs allow us to check any boolean expression, while switch-case always compares only the variable with the exact value. Therefore, the same conditions as for if-else, such as "greater-than," are not allowed for switch-case.

The syntax for the switch-case statements looks like this:

```bash
case <expression> in
    pattern_1 ) statements ;;
    pattern_2 ) statements ;;
    pattern_3 ) statements ;;
esac
```

The definition of switch-case starts with case, followed by the variable or value as an expression, which is then compared in the pattern. If the variable or value matches the expression, then the statements are executed after the parenthesis and ended with a double semicolon (;;).

In our CIDR.sh script, we have used such a case statement. Here we defined four different options that we assigned to our script, how it should proceed after our decision.

```bash
# Available options
echo -e "Additional options available:"
echo -e "\t1) Identify the corresponding network range of target domain."
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

With the first two options, this script executes different functions that we had defined before. With the third option, both functions are executed, and with any other option, the script will be terminated.
