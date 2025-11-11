# Module 1: File & Directory Management

This module covers essential Linux commands for managing files and directories from the command line.


## Table of Contents

1. [pwd – Print Working Directory](#pwd--print-working-directory)
2. [ls – List Files and Directories](#ls--list-files-and-directories)
3. [cd – Change Directory](#cd--change-directory)
4. [mkdir – Make Directory](#mkdir--make-directory)
5. [rmdir – Remove Empty Directory](#rmdir--remove-empty-directory)
6. [rm – Remove Files or Directories](#rm--remove-files-or-directories)
7. [cp – Copy Files or Directories](#cp--copy-files-or-directories)
8. [mv – Move or Rename Files or Directories](#mv--move-or-rename-files-or-directories)
9. [Common Flags and Options](#common-flags-and-options)



## pwd – Print Working Directory

The `pwd` command prints the current working directory (i.e., where you are located in the filesystem).

### Example:
```bash
pwd
```
**Output:**
```
/home/kali/Desktop
```

---

## ls – List Files and Directories

The `ls` command lists files and directories in the current location.

### Common Options:
- `ls` – Lists non-hidden files.
- `ls -a` – Lists all files, including hidden ones (files starting with `.`).
- `ls -l` – Lists files in long format (shows permissions, owner, size, date).
- `ls -lh` – Human-readable file sizes (e.g., KB, MB).
- `ls -lt` – Sorts files by time (newest first).

### Example:
```bash
ls -lh
```

---

## cd – Change Directory

The `cd` command changes your current directory.

### Examples:
```bash
cd Documents      # Moves into the Documents folder
cd ..             # Goes up one directory level
cd /home/kali/Desktop  # Goes directly to the Desktop
cd ~/Desktop      # Same as above (~ = home directory shortcut)
```

---

## mkdir – Make Directory

The `mkdir` command creates a new directory.

### Examples:
```bash
mkdir test        # Creates a folder named "test"
mkdir -p folder1/folder2/folder3  # Creates nested directories
```

**Note:** The `-p` flag allows you to create parent directories as needed.

---

## rmdir – Remove Empty Directory

The `rmdir` command deletes an empty directory.

### Example:
```bash
rmdir test        # Deletes the "test" folder (only if empty)
```

---

## rm – Remove Files or Directories

The `rm` command deletes files or directories.

### Examples:
```bash
rm file.txt              # Deletes a file
rm -f file.txt           # Forces deletion without confirmation
rm -r foldername         # Recursively deletes a folder and its contents
rm -rf foldername        # Forces recursive deletion (use with caution!)
```

> ⚠️ **Warning:** `rm -rf` is a powerful and dangerous command. It deletes files/folders **without confirmation**.

---

## cp – Copy Files or Directories

The `cp` command copies files or directories.

### Examples:
```bash
cp file1.txt file2.txt                 # Copies file1 to file2
cp file.txt /home/kali/Desktop/        # Copies file to Desktop
cp -r folder/ /home/kali/Documents/    # Copies a folder and its contents
```

### Common Options:
- `-r` – Recursive (for copying directories)
- `-i` – Interactive (asks before overwriting)
- `-v` – Verbose (shows progress)

### Example with Options:
```bash
cp -iv file1.txt file2.txt
```
This will prompt before overwriting and show progress.

---

## mv – Move or Rename Files or Directories

The `mv` command moves or renames files or directories.

### Examples:
```bash
mv file.txt /home/kali/Desktop/   # Moves file to Desktop
mv oldname.txt newname.txt       # Renames file
mv folder1/ folder2/             # Moves folder1 inside folder2
```

---

## Common Flags and Options

| Flag | Meaning (English)                      | Example Usage     |
|------|----------------------------------------|-------------------|
| `-a` | Show all (including hidden files)      | `ls -a`           |
| `-l` | Long format (detailed view)            | `ls -l`           |
| `-r` | Recursive (applies to subdirectories)  | `rm -r folder`    |
| `-f` | Force (skip confirmation prompts)      | `rm -f file`      |
| `-y` | Assume "yes" to all prompts            | `apt -y install`  |
| `-t` | Sort by time or type (context-based)   | `ls -lt`          |
| `-i` | Interactive (prompt before overwriting)| `cp -i file.txt`  |
| `-v` | Verbose (show detailed output)         | `cp -v file.txt`  |

---

## Summary

These commands are the foundation of navigating and managing files and directories in a Linux environment. Mastering them will help you work more efficiently in the terminal.
