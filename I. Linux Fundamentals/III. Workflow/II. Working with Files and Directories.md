# Working with Files and Directories

## Create, Move, Copy, and Delete

### 1. Create Empty File (touch):

```bash
touch info.txt
```

Creates an empty file named `info.txt`.

### 2. Create Directory (mkdir):

```bash
mkdir Storage
```

Creates a directory named `Storage`.

### 3. Create Nested Directories:

```bash
mkdir -p Storage/local/user/documents
```

Creates nested directories using the `-p` option.

### 4. View Directory Structure (tree):

```bash
tree .
```

Displays the directory structure.

### 5. Create File in a Specific Directory:

```bash
touch ./Storage/local/user/userinfo.txt
```

Creates a file directly in a specified directory.

### 6. Move/Rename File or Directory (mv):

```bash
mv info.txt information.txt
```

Renames the file from `info.txt` to `information.txt`.

### 7. Move File to a Directory:

```bash
mv information.txt Storage/
```

Moves the file to the `Storage` directory.

### 8. Create and Move Multiple Files:

```bash
touch readme.txt
mv information.txt readme.txt Storage/
```

Creates a file `readme.txt` and moves both `information.txt` and `readme.txt` to the `Storage` directory.

### 9. Copy File to a Directory (cp):

```bash
cp Storage/readme.txt Storage/local/
```

Copies the file `readme.txt` to the `local` directory within the `Storage` directory.

### 10. View Updated Directory Structure:

```bash
tree .
```

Displays the updated directory structure.

These commands allow you to efficiently create, move, rename, and copy files and directories in Linux. Experiment with these commands to enhance your proficiency. 📁💻
