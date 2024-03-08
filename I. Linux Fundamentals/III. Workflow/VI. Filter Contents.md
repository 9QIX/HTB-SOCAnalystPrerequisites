# Filtering Contents in Linux

In this section, we will explore various tools in Linux that help filter and process the contents of files. These tools include `more`, `less`, `head`, `tail`, `sort`, `grep`, `cut`, `tr`, `column`, `awk`, `sed`, and `wc`.

## 1. Using `more` and `less`

```bash
# Using more
$ more /etc/passwd

# Using less
$ less /etc/passwd
```

## 2. Using `head` and `tail`

```bash
# Display the first ten lines of a file
$ head /etc/passwd

# Display the last ten lines of a file
$ tail /etc/passwd
```

## 3. Using `sort`

```bash
# Sort the content of /etc/passwd alphabetically
$ cat /etc/passwd | sort
```

## 4. Using `grep`

```bash
# Display lines containing "/bin/bash"
$ cat /etc/passwd | grep "/bin/bash"

# Exclude lines containing "false" or "nologin"
$ cat /etc/passwd | grep -v "false\|nologin"
```

## 5. Using `cut`

```bash
# Display the first field (username) of /etc/passwd
$ cat /etc/passwd | cut -d":" -f1
```

## 6. Using `tr`

```bash
# Replace ":" with space in /etc/passwd
$ cat /etc/passwd | tr ":" " "
```

## 7. Using `column`

```bash
# Display in tabular form
$ cat /etc/passwd | tr ":" " " | column -t
```

## 8. Using `awk`

```bash
# Display the first and last fields (username and shell)
$ cat /etc/passwd | tr ":" " " | awk '{print $1, $NF}'
```

## 9. Using `sed`

```bash
# Replace "bin" with "HTB" in /etc/passwd
$ cat /etc/passwd | tr ":" " " | sed 's/bin/HTB/g'
```

## 10. Using `wc`

```bash
# Count the number of lines in the filtered output
$ cat /etc/passwd | grep -v "false\|nologin" | wc -l
```

## Practice Exercises

1. Display the line with the username "cry0l1t3."
2. Display only the usernames.
3. Display the username "cry0l1t3" and his UID.
4. Display the username "cry0l1t3" and his UID separated by a comma (,).
5. Display the username "cry0l1t3," his UID, and the set shell separated by a comma (,).
6. Display all usernames with their UID and set shells separated by a comma (,).
7. Display all usernames with their UID and set shells separated by a comma (,) and exclude those containing "nologin" or "false."
8. Display all usernames with their UID and set shells separated by a comma (,) and exclude those containing "nologin." Count all lines in the filtered output.

### Answers

### 1. A line with the username `cry0l1t3`:

```bash
grep 'cry0l1t3' /etc/passwd
```

### 2. The usernames:

```bash
cut -d: -f1 /etc/passwd
```

### 3. The username `cry0l1t3` and his UID:

```bash
grep 'cry0l1t3' /etc/passwd | cut -d: -f1,3
```

### 4. The username `cry0l1t3` and his UID separated by a comma (,):

```bash
grep 'cry0l1t3' /etc/passwd | cut -d: -f1,3 | tr ':' ','
```

### 5. The username `cry0l1t3`, his UID, and the set shell separated by a comma (,):

```bash
grep 'cry0l1t3' /etc/passwd | cut -d: -f1,3,7 | tr ':' ','
```

### 6. All usernames with their UID and set shells separated by a comma (,):

```bash
cut -d: -f1,3,7 /etc/passwd | tr ':' ','
```

### 7. All usernames with their UID and set shells separated by a comma (,) and exclude the ones that contain `nologin` or `false`:

```bash
cut -d: -f1,3,7 /etc/passwd | grep -vE 'nologin|false' | tr ':' ','
```

### 8. All usernames with their UID and set shells separated by a comma (,) and exclude the ones that contain `nologin` and count all lines of the filtered output:

```bash
cut -d: -f1,3,7 /etc/passwd | grep -v 'nologin' | grep -v 'false' | tr ':' ',' | wc -l
```

Feel free to run these commands in your Linux terminal to observe the output for each exercise. It's a great way to practice and become more familiar with command-line tools.
