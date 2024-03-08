# Regular Expressions

## Grouping Operators

- `()` : The round brackets are used to group parts of a regex. Within the brackets, you can define further patterns which should be processed together.

- `[]` : The square brackets are used to define character classes. Inside the brackets, you can specify a list of characters to search for.

- `{1,10}` : The curly brackets are used to define quantifiers. Inside the brackets, you can specify a number or a range that indicates how often a previous pattern should be repeated.

- `|` : Also called the OR operator and shows results when one of the two expressions matches.

- `.*` : Also called the AND operator and displays results only if both expressions match.

### OR Operator Example:

- `grep -E "(my|false)" /etc/passwd`

  In this case, you are searching for lines in `/etc/passwd` that contain either "my" or "false". Since at least one of these patterns occurs in each of the three lines, all three lines are displayed.

### AND Operator Example:

- `grep -E "(my.*false)" /etc/passwd`

  Here, you are searching for lines that contain both "my" and "false". The line containing "mysql" is the only one that satisfies this condition, so only that line is displayed.

### Simplified Example using Two Separate grep Commands:

- `grep -E "my" /etc/passwd | grep -E "false"`

  This achieves the same result as the AND operator example. The first `grep` command filters lines containing "my", and then the second `grep` command filters lines containing "false" from the output of the first command. The result is the same line containing "mysql".

### Cheat Sheet

Certainly! Here are the tables without the "Try it" parts:

### Metacharacters

| Character | Description                                 | Example        |
| --------- | ------------------------------------------- | -------------- |
| []        | A set of characters                         | "[a-m]"        |
| \         | Signals a special sequence                  | "\d"           |
| .         | Any character (except newline character)    | "he..o"        |
| ^         | Starts with                                 | "^hello"       |
| $         | Ends with                                   | "planet$"      |
| \*        | Zero or more occurrences                    | "he.\*o"       |
| +         | One or more occurrences                     | "he.+o"        |
| ?         | Zero or one occurrences                     | "he.?o"        |
| {}        | Exactly the specified number of occurrences | "he.{2}o"      |
| \|        | Either or                                   | "falls\|stays" |
| ()        | Capture and group                           |                |

### Special Sequences

| Character | Description                                                                                                                                                                                                 | Example              |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| \A        | Returns a match if the specified characters are at the beginning of the string                                                                                                                              | "\AThe"              |
| \b        | Returns a match where the specified characters are at the beginning or at the end of a word (the "r" in the beginning is making sure that the string is being treated as a "raw string")                    | r"\bain" or r"ain\b" |
| \B        | Returns a match where the specified characters are present, but NOT at the beginning (or at the end) of a word (the "r" in the beginning is making sure that the string is being treated as a "raw string") | r"\Bain" or r"ain\B" |
| \d        | Returns a match where the string contains digits (numbers from 0-9)                                                                                                                                         | "\d"                 |
| \D        | Returns a match where the string DOES NOT contain digits                                                                                                                                                    | "\D"                 |
| \s        | Returns a match where the string contains a white space character                                                                                                                                           | "\s"                 |
| \S        | Returns a match where the string DOES NOT contain a white space character                                                                                                                                   | "\S"                 |
| \w        | Returns a match where the string contains any word characters (characters from a to Z, digits from 0-9, and the underscore \_ character)                                                                    | "\w"                 |
| \W        | Returns a match where the string DOES NOT contain any word characters                                                                                                                                       | "\W"                 |
| \Z        | Returns a match if the specified characters are at the end of the string                                                                                                                                    | "Spain\Z"            |

### Sets

| Set       | Description                                                                       |
|-----------|-----------------------------------------------------------------------------------|
| [arn]     | Returns a match where one of the specified characters (a, r, or n) is present    |
| [a-n]     | Returns a match for any lowercase character, alphabetically between a and n       |
| [^arn]    | Returns a match for any character EXCEPT a, r, and n                               |
| [0123]    | Returns a match where any of the specified digits (0, 1, 2, or 3) are present    |
| [0-9]     | Returns a match for any digit between 0 and 9                                    |
| [0-5][0-9]| Returns a match for any two-digit numbers from 00 and 59                         |
| [a-zA-Z]  | Returns a match for any character alphabetically between a and z, lowercase OR uppercase |
| [+]       | In sets, +, *, ., |, (), $, {} have no special meaning, so [+] means: return a match for any + character in the string |

## Regular Expressions Practice

Let's practice regular expressions using the `/etc/ssh/sshd_config` file on the Pwnbox instance.

1. Show all lines that do not contain the `#` character.

```bash
$ grep -E -v "#" /etc/ssh/sshd_config | awk NF
```

2. Search for all lines that contain a word that starts with `Permit`.

```bash
$ grep -E "Permit\w+" /etc/ssh/sshd_config
```

3. Search for all lines that contain a word ending with `Authentication`.

```bash
$ grep -E "\w+Authentication" /etc/ssh/sshd_config
```

4. Search for all lines containing the word `Key`.

```bash
$ grep -E "Key" /etc/ssh/sshd_config
```

5. Search for all lines beginning with `Password` and containing `yes`.

```bash
$ grep -E "^Password.*yes" /etc/ssh/sshd_config
```

6. Search for all lines that end with `yes`.

```bash
$ grep -E "yes$" /etc/ssh/sshd_config
```

These commands use the extended regular expression (`-E`) option with `grep` to perform the specified pattern matching in the `/etc/ssh/sshd_config` file. Each command corresponds to one of the exercises, and you can modify them as needed for further exploration.
