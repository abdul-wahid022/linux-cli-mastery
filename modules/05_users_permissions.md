# Module 5: Users & Permissions

**Objective:** Understand who can do what to a file, change permissions and ownership, and manage users, groups and `sudo`.

| Section | Topic |
| ------- | ----- |
| [1](#1-the-permission-model) | The permission model |
| [2](#2-reading-permissions) | Reading `ls -l` |
| [3](#3-changing-permissions-chmod) | `chmod`: symbolic and numeric |
| [4](#4-default-permissions-umask) | `umask` |
| [5](#5-changing-ownership) | `chown` `chgrp` |
| [6](#6-special-permissions) | SUID, SGID, sticky bit |
| [7](#7-user-management) | Users |
| [8](#8-group-management) | Groups |
| [9](#9-sudo-and-root) | `sudo` and root |
| [10](#10-important-files) | `/etc/passwd` and friends |
| [11](#practice) | Practice, common mistakes |

---

## 1. The permission model

Every file and directory has an **owner**, a **group** and three sets of permissions.

| Category | Symbol | Who |
| -------- | ------ | --- |
| Owner (user) | `u` | The account that owns the file |
| Group | `g` | Members of the file's group |
| Others | `o` | Everyone else |

| Permission | Symbol | On a file | On a directory |
| ---------- | ------ | --------- | -------------- |
| Read | `r` | View the contents | List the names inside |
| Write | `w` | Modify the contents | Create, rename and delete entries |
| Execute | `x` | Run it as a program | Enter it with `cd` |

---

## 2. Reading permissions

```bash
ls -l
```
```
-rwxr-xr-- 1 kali kali 1024 Aug 23 test.txt
```

| Part | Value | Meaning |
| ---- | ----- | ------- |
| 1st character | `-` | File type: `-` file, `d` directory, `l` symbolic link |
| Characters 2-4 | `rwx` | Owner: read, write, execute |
| Characters 5-7 | `r-x` | Group: read and execute |
| Characters 8-10 | `r--` | Others: read only |
| Next | `1` | Number of hard links |
| `kali kali` | | Owner and group |
| `1024` | | Size in bytes |

---

## 3. Changing permissions (chmod)

### Symbolic mode

Format: `chmod [who][operator][permissions] file`

| Who | Operator | Permissions |
| --- | -------- | ----------- |
| `u` owner, `g` group, `o` others, `a` all | `+` add, `-` remove, `=` set exactly | `r` `w` `x` |

| Command | Result |
| ------- | ------ |
| `chmod +x script.sh` | Add execute for everyone |
| `chmod u+x script.sh` | Add execute for the owner only |
| `chmod g-w file.txt` | Remove write from the group |
| `chmod o+r file.txt` | Add read for others |
| `chmod u+rwx,g+rx,o+r file.txt` | Owner `rwx`, group `r-x`, others `r--` |
| `chmod -R u+rwX folder/` | Apply recursively (`X` adds execute only to directories) |

### Numeric mode

Each permission has a value: **r = 4, w = 2, x = 1**. Add them for each category.

| Number | Permissions | Meaning |
| ------ | ----------- | ------- |
| 0 | `---` | None |
| 1 | `--x` | Execute |
| 2 | `-w-` | Write |
| 3 | `-wx` | Write and execute |
| 4 | `r--` | Read |
| 5 | `r-x` | Read and execute |
| 6 | `rw-` | Read and write |
| 7 | `rwx` | Everything |

Three digits give owner, group and others: `chmod 754 file` is `rwx r-x r--`.

| Mode | Result | Typical use |
| ---- | ------ | ----------- |
| `644` | `rw-r--r--` | Regular files |
| `755` | `rwxr-xr-x` | Scripts, programs, directories |
| `600` | `rw-------` | Private files, SSH keys |
| `700` | `rwx------` | Private directories and scripts |
| `770` | `rwxrwx---` | Shared with a group, closed to others |
| `777` | `rwxrwxrwx` | Everyone can do everything. Avoid. |

---

## 4. Default permissions (umask)

`umask` sets which permissions are **removed** from new files and directories.

| umask | New files | New directories |
| ----- | --------- | --------------- |
| `022` (common default) | `644` | `755` |
| `077` | `600` | `700` |

```bash
umask          # show current value
umask 077      # for this shell session only
```

---

## 5. Changing ownership

| Command | What it does |
| ------- | ------------ |
| `sudo chown ali file.txt` | Change the owner |
| `sudo chown ali:developers file.txt` | Change owner and group |
| `sudo chown -R ali folder/` | Change recursively |
| `sudo chgrp developers file.txt` | Change the group |
| `sudo chgrp -R developers project/` | Change the group recursively |

---

## 6. Special permissions

| Bit | Number | Symbol | Effect |
| --- | ------ | ------ | ------ |
| SUID | 4 | `s` in owner position | A program runs with the **owner's** privileges |
| SGID | 2 | `s` in group position | A program runs with the group's privileges. On a directory, new files inherit the group |
| Sticky | 1 | `t` at the end | In a shared directory, only the owner can delete their own files |

| Command | Result |
| ------- | ------ |
| `chmod u+s file` or `chmod 4755 file` | Set SUID |
| `chmod g+s dir/` or `chmod 2775 dir/` | Set SGID |
| `chmod +t dir/` or `chmod 1777 dir/` | Set the sticky bit (as on `/tmp`) |

`/usr/bin/passwd` uses SUID so normal users can change their password, which edits a root-owned file.

---

## 7. User management

| Command | What it does |
| ------- | ------------ |
| `whoami` | Current user name |
| `id` | UID, GID and groups of the current user |
| `id ali` | Same for another user |
| `who`, `w` | Who is logged in |
| `last` | Login history |
| `sudo adduser ali` | Create a user interactively (home directory, password prompt) |
| `sudo useradd -m -s /bin/bash ali` | Create a user in one command (low-level tool) |
| `sudo passwd ali` | Set or change a password |
| `sudo passwd -l ali` | Lock an account |
| `sudo chage -l ali` | Show password ageing settings |
| `sudo usermod -aG sudo ali` | Add a user to the `sudo` group |
| `sudo deluser --remove-home ali` | Delete a user and the home directory |
| `sudo userdel -r ali` | Same with the low-level tool |
| `su ali` | Switch to another user |
| `su -` | Switch to root with root's environment |

`adduser` is the friendly Debian wrapper, `useradd` is the low-level tool that needs more options.

```bash
id
```
```
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

---

## 8. Group management

| Command | What it does |
| ------- | ------------ |
| `groups` | Groups of the current user |
| `groups ali` | Groups of another user |
| `sudo groupadd developers` | Create a group |
| `sudo groupdel developers` | Delete a group |
| `sudo usermod -aG developers ali` | Add a user to a group |
| `sudo gpasswd -d ali developers` | Remove a user from a group |

`-a` appends. Without it, `usermod -G` **replaces** all supplementary groups. The user must log out and in again before a new group applies.

---

## 9. sudo and root

`sudo` runs one command with root privileges. Users in the `sudo` group may use it.

| Command | What it does |
| ------- | ------------ |
| `sudo <command>` | Run one command as root |
| `sudo -l` | List what you are allowed to run |
| `sudo -i` | Open a root shell |
| `sudo !!` | Repeat the previous command with sudo |
| `sudo visudo` | Safely edit `/etc/sudoers` (checks syntax) |

```bash
ls /root          # Permission denied
sudo ls /root     # works
```

---

## 10. Important files

| File | Contents |
| ---- | -------- |
| `/etc/passwd` | One line per user: name, UID, GID, home, shell |
| `/etc/shadow` | Password hashes, readable only by root |
| `/etc/group` | Groups and their members |
| `/etc/sudoers` | Who may use sudo |

```bash
cat /etc/passwd | head -n 3
```
```
root:x:0:0:root:/root:/usr/bin/zsh
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

---

## Practice

1. Create `script.sh` that prints `Hello`, make it executable and run it.
2. Set `notes.txt` to `600` and confirm with `ls -l`.
3. Create the group `developers` and the user `ali`, and add `ali` to the group.
4. Create `/srv/project`, give it to the group `developers` and set mode `2770`. Create a file inside and check which group it gets.
5. Show the groups of `ali` with `id ali`.
6. List files under `/home` that are world-writable: `find /home -type f -perm -002`.
7. List SUID programs: `find / -perm -4000 -type f 2>/dev/null`.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| `chmod 777` to "fix" a problem | Anyone can modify or replace the file | Find the real cause, grant the minimum |
| `chmod -R 777 /` or `chmod -R` on the wrong path | Can break the whole system | Check the path, avoid `-R` on system directories |
| `usermod -G` without `-a` | Removes the user from all other groups | Always `usermod -aG` |
| Group change does not apply | The session still has the old groups | Log out and in again |
| SSH key with mode `644` | SSH refuses to use it | `chmod 600 ~/.ssh/id_rsa` |
| Editing `/etc/sudoers` with a normal editor | A syntax error can lock out sudo | Use `visudo` |

---

[Previous: System & Processes](../04-system-and-processes/README.md) · [Back to index](../../README.md) · [Next: System Logs](../06-system-logs/README.md)
