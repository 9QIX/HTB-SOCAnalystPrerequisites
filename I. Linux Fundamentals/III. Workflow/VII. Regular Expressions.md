# Regular Expressions Practice

Let's practice regular expressions using the `/etc/ssh/sshd_config` file on the Pwnbox instance.

## Grouping Operators

- `()` : The round brackets are used to group parts of a regex. Within the brackets, you can define further patterns which should be processed together.

- `[]` : The square brackets are used to define character classes. Inside the brackets, you can specify a list of characters to search for.

- `{1,10}` : The curly brackets are used to define quantifiers. Inside the brackets, you can specify a number or a range that indicates how often a previous pattern should be repeated.

- `|` : Also called the OR operator and shows results when one of the two expressions matches.

- `.*` : Also called the AND operator and displays results only if both expressions match.

1. Show all lines that do not contain the `#` character.

```bash
$ grep -E -v "#" /etc/ssh/sshd_config
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
