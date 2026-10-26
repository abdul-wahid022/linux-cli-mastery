# Module 1: Files & Directories

**Objective:** Move around the filesystem and create, copy, move, find and delete files and directories from the command line.

| Section | Topic |
| ------- | ----- |
| [1](#1-the-linux-filesystem) | The Linux filesystem |
| [2](#2-navigation) | Navigation: `pwd` `ls` `cd` |
| [3](#3-creating-files-and-directories) | Creating: `mkdir` `touch` `cat` `echo` |
| [4](#4-copying-moving-and-deleting) | Copying, moving, deleting: `cp` `mv` `rm` `rmdir` |
| [5](#5-finding-and-inspecting) | Finding and inspecting: `find` `file` `stat` `tree` `which` |
| [6](#6-wildcards-and-links) | Wildcards and links |
| [7](#7-getting-help) | Getting help |
| [8](#8-common-flags) | Common flags |
| [9](#practice) | Practice, common mistakes |

---

## 1. The Linux filesystem

Everything starts at the root directory `/`. There are no drive letters.

| Directory | Contents |
| --------- | -------- |
| `/` | Root of the filesystem |
| `/home` | Home directories of users (`/home/kali`) |
| `/root` | Home directory of the root user |
| `/etc` | System configuration files |
| `/var` | Variable data: logs, caches, spool files |
| `/tmp` | Temporary files, cleared on reboot |
| `/usr` | Installed programs and libraries |
| `/bin`, `/sbin` | Essential commands (user and system) |
| `/opt` | Optional third-party software |
| `/dev` | Device files |
| `/proc` | Live information about processes and the kernel |

**Absolute and relative paths**

| Type | Starts with | Example |
| ---- | ----------- | ------- |
| Absolute | `/` | `/home/kali/Documents/notes.txt` |
| Relative | anything else | `Documents/notes.txt` (from the current directory) |

Special names: `.` is the current directory, `..` is the parent directory, `~` is your home directory.

Files whose names start with a dot (`.bashrc`) are hidden. Show them with `ls -a`.

---

## 2. Navigation

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `pwd` | Print the current directory | `pwd` |
| `ls` | List files | `ls` |
| `ls -a` | Include hidden files | `ls -a` |
| `ls -l` | Long format: permissions, owner, size, date | `ls -l` |
| `ls -lh` | Long format with readable sizes (KB, MB) | `ls -lh` |
| `ls -lt` | Sort by modification time, newest first | `ls -lt` |
| `ls -R` | List subdirectories recursively | `ls -R` |
| `ls -d */` | List only directories | `ls -d */` |
| `cd <dir>` | Change directory | `cd Documents` |
| `cd ..` | Go up one level | `cd ..` |
| `cd ~` or `cd` | Go to the home directory | `cd ~/Desktop` |
| `cd -` | Go back to the previous directory | `cd -` |

```bash
pwd
```
```
/home/kali/Desktop
```

---

## 3. Creating files and directories

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `mkdir <name>` | Create a directory | `mkdir test` |
| `mkdir -p a/b/c` | Create nested directories, parents as needed | `mkdir -p folder1/folder2/folder3` |
| `mkdir dir/{a,b,c}` | Create several directories at once (brace expansion) | `mkdir project/{src,docs,tests}` |
| `touch <file>` | Create an empty file, or update its timestamp | `touch notes.txt` |
| `echo "text" > file` | Write text to a file (overwrites) | `echo "hello" > notes.txt` |
| `echo "text" >> file` | Append text to a file | `echo "more" >> notes.txt` |
| `cat > file` | Type content, finish with `Ctrl+D` | `cat > notes.txt` |

---

## 4. Copying, moving and deleting

### Copy: `cp`

| Command | What it does |
| ------- | ------------ |
| `cp file1 file2` | Copy a file |
| `cp file ~/Desktop/` | Copy a file into a directory |
| `cp -r folder/ ~/Documents/` | Copy a directory and its contents |
| `cp -i file1 file2` | Ask before overwriting |
| `cp -v file1 file2` | Print each file as it is copied |
| `cp -a folder/ backup/` | Archive mode: keeps permissions, owners and timestamps |

### Move and rename: `mv`

| Command | What it does |
| ------- | ------------ |
| `mv file.txt ~/Desktop/` | Move a file |
| `mv old.txt new.txt` | Rename a file |
| `mv folder1/ folder2/` | Move `folder1` inside `folder2` if `folder2` exists, otherwise rename it |

### Delete: `rmdir` and `rm`

| Command | What it does |
| ------- | ------------ |
| `rmdir test` | Delete an empty directory |
| `rm file.txt` | Delete a file |
| `rm -i file.txt` | Ask before each deletion |
| `rm -r folder` | Delete a directory and everything in it |
| `rm -rf folder` | Same, never asks, ignores missing files |

> **Warning:** there is no recycle bin. `rm` deletes immediately. Check the path before you press Enter, and never run `rm -rf` on a path you built from a variable you have not printed first.

---

## 5. Finding and inspecting

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `find <dir> -name "<pattern>"` | Find by name | `find ~ -name "*.txt"` |
| `find <dir> -type d` | Find directories (`-type f` for files) | `find . -type d` |
| `find <dir> -size +10M` | Find files larger than 10 MB | `find / -size +10M 2>/dev/null` |
| `find <dir> -mtime -1` | Modified in the last 24 hours | `find . -mtime -1` |
| `file <name>` | Detect the file type | `file notes.txt` |
| `stat <name>` | Size, permissions, timestamps | `stat notes.txt` |
| `which <command>` | Show where a command lives | `which python3` |
| `tree` | Show a directory tree (`sudo apt install tree` if missing) | `tree -L 2` |

---

## 6. Wildcards and links

**Wildcards** let one pattern match many names.

| Pattern | Matches | Example |
| ------- | ------- | ------- |
| `*` | Any number of characters | `ls *.txt` |
| `?` | Exactly one character | `ls file?.txt` |
| `[abc]` | One character from the set | `ls file[12].txt` |

**Symbolic links** are shortcuts to another file or directory.

```bash
ln -s /var/log/syslog ~/syslog-link
ls -l ~/syslog-link
```

---

## 7. Getting help

| Command | What it does |
| ------- | ------------ |
| `man <command>` | Full manual (quit with `q`) |
| `<command> --help` | Short usage summary |
| `type <command>` | Shows whether it is a program, alias or shell builtin |

Useful shell shortcuts: `Tab` completes names, `Ctrl+C` cancels, `Ctrl+L` clears the screen, `Ctrl+R` searches history, `!!` repeats the last command, `history` lists past commands.

---

## 8. Common flags

| Flag | Meaning | Example |
| ---- | ------- | ------- |
| `-a` | All (include hidden files) | `ls -a` |
| `-l` | Long format | `ls -l` |
| `-h` | Human-readable sizes | `ls -lh` |
| `-r`, `-R` | Recursive | `rm -r folder`, `ls -R` |
| `-f` | Force, no prompts | `rm -f file` |
| `-i` | Interactive, prompt before overwriting or deleting | `cp -i a b` |
| `-v` | Verbose, print what is being done | `cp -v a b` |
| `-t` | Sort by time (with `ls`) | `ls -lt` |
| `-p` | Create parents (with `mkdir`) | `mkdir -p a/b/c` |

---

## Practice

1. Go to your home directory and print where you are.
2. Create this structure with one command: `project/src`, `project/docs`, `project/tests`.
3. Create `project/docs/notes.txt` with the text `first note`, then append a second line.
4. Copy `project/docs/notes.txt` to `project/src/` and rename the copy to `copy.txt`.
5. Use `find` to list every `.txt` file under `project`.
6. Delete the `project` directory and confirm it is gone with `ls`.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| `rm -rf /some/path /` with a stray space | Deletes far more than intended | Check the command before Enter; use `rm -ri` while learning |
| `cp folder/ dest/` without `-r` | `omitting directory` error | Add `-r` |
| `mv a b` where `b` already exists | `b` is silently overwritten | Use `mv -i` |
| `rmdir` on a non-empty directory | `Directory not empty` | Use `rm -r` |
| Spaces in names (`my file.txt`) | Treated as two arguments | Quote it: `"my file.txt"` |

---

[Back to index](../../README.md) · [Next: Compression & Archiving](../02-compression-and-archiving/README.md)
