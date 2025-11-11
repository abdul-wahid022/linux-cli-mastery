# Module 5: Linux Users & Permissions

Understanding Linux users and permissions is crucial for system security and file management. In Linux, every file and directory has specific permissions that control who can read, write, or execute them. This module covers user management, file permissions, and access control - essential skills for any Linux administrator or user who wants to properly secure their system.

### Table of Contents

1.  [What are Users & Permissions?](#what-are-users--permissions)
2.  [Understanding Linux Permissions](#understanding-linux-permissions)
3.  [Permission Types (rwx)](#permission-types-rwx)
4.  [User Categories (Owner, Group, Others)](#user-categories-owner-group-others)
5.  [Viewing Permissions](#viewing-permissions)
6.  [Changing Permissions (chmod)](#changing-permissions-chmod)
7.  [Numeric Permission System](#numeric-permission-system)
8.  [Changing Ownership (chown, chgrp)](#changing-ownership-chown-chgrp)
9.  [User Management Commands](#user-management-commands)
10. [Group Management](#group-management)
11. [Sudo and Root Access](#sudo-and-root-access)
12. [Practical Examples](#practical-examples)
13. [Common Use Cases](#common-use-cases)
14. [Best Practices](#best-practices)
15. [Conclusion](#conclusion)

---

## What are Users & Permissions?

In Linux, every file and directory is owned by a user and belongs to a group. Permissions determine what actions different users can perform on files and directories. This security model ensures that:
- Users can only access files they're authorized to use
- System files are protected from accidental modification
- Multiple users can share a system safely
- Sensitive data remains private

**Key Concepts:**
- **Users:** Individual accounts on the system
- **Groups:** Collections of users with shared permissions
- **Permissions:** Rules defining who can read, write, or execute files
- **Ownership:** Every file has an owner and a group

---

## Understanding Linux Permissions

### Real-Life Example (Ghar ki misaal)

**🏠 Imagine your house (computer) has a room (file/folder):**

Think of a file or folder like a room in your house. There are 3 types of people who can control that room:

1. **Owner (User)** → You are the owner of the house
   - **Example:** You created the file, so you're the owner
   - You have full control

2. **Group** → A small team or your friends who have access
   - **Example:** You gave your 3 friends keys to enter the room
   - They can also access the room
   - These people belong to the same "group"

3. **Others (World)** → Everyone else in the neighborhood
   - **Example:** Strangers or other people who are not in your group
   - If you allow them, they can also enter

---

## Permission Types (rwx)

Every file or folder in Linux has 3 types of permissions:

| Permission | Symbol | Explanation (English) | Explanation (Urdu) |
| :--- | :--- | :--- | :--- |
| **Read** | `r` | View file content or list folder contents | File ko padhna / folder ki list dekhna |
| **Write** | `w` | Modify file or add/delete files in folder | File me likhna / folder me cheezen add ya delete karna |
| **Execute** | `x` | Run file as program / enter folder | File ko program ke taur pe chalana / folder me enter karna |

---

## User Categories (Owner, Group, Others)

Linux divides users into three categories for permission control:

| Category | Explanation (English) | Explanation (Urdu) |
| :--- | :--- | :--- |
| **Owner** | The person who created the file | Jisne file banayi hai, wo hi owner hai |
| **Group** | A team or collection of users with shared access | Ek chhoti team ya doston ka group, jinke paas ek hi permission hai |
| **Others** | Everyone else on the system | Baaki sab jo na tum ho, na tumhare group wale |

---

## Viewing Permissions

### Using ls -l

The `ls -l` command shows detailed file information including permissions.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `ls -l` | Show file permissions and details | File ke owner, group aur others ke liye kya permissions hain | `ls -l` |
| `ls -la` | Show all files including hidden ones | Hidden files bhi dikhao with permissions | `ls -la` |

---

### Example: Viewing File Permissions

**Command:**
```bash
ls -l
```

**👉 Output:**
```
-rwxr-xr-- 1 kali kali 1024 Aug 23 test.txt
```

**Breaking Down the Output:**

```
-rwxr-xr-- 1 kali kali 1024 Aug 23 test.txt
│└─┬─┘└┬┘└┬┘
│  │   │  └─── Others permissions (r--)
│  │   └────── Group permissions (r-x)
│  └────────── Owner permissions (rwx)
└───────────── File type (- = file, d = directory)
```

**Detailed Explanation:**

| Part | Symbol | Meaning (English) | Meaning (Urdu) |
| :--- | :--- | :--- | :--- |
| First character | `-` | Regular file (d = directory) | Ye ek file hai (folder hota to `d` hota) |
| Next 3 chars | `rwx` | Owner permissions (read, write, execute) | Owner ke liye: padhna, likhna, chalana (sab allowed) |
| Next 3 chars | `r-x` | Group permissions (read, execute, no write) | Group ke liye: padhna aur chalana allowed, likhna nahi |
| Last 3 chars | `r--` | Others permissions (read only) | Others ke liye: sirf padhna allowed |

---

## Changing Permissions (chmod)

The `chmod` (Change Mode) command is used to change file and directory permissions.

### Basic chmod Commands

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `chmod u+x file` | Add execute permission for owner | Owner ke liye execute permission add karo | `chmod u+x script.sh` |
| `chmod g-w file` | Remove write permission for group | Group se write permission hata do | `chmod g-w file.txt` |
| `chmod o+r file` | Add read permission for others | Others ke liye read permission add karo | `chmod o+r file.txt` |
| `chmod 777 file` | Give full permissions to everyone | Sabko (Owner, Group, Others) full access do | `chmod 777 file.txt` |
| `chmod 755 file` | Common permission for scripts | Owner ko full, others ko read+execute | `chmod 755 script.sh` |
| `chmod 644 file` | Common permission for files | Owner ko read+write, others ko sirf read | `chmod 644 file.txt` |

---

### Permission Symbols

| Symbol | Meaning | Explanation (Urdu) |
| :--- | :--- | :--- |
| `u` | User (owner) | File ka malik |
| `g` | Group | Group wale log |
| `o` | Others | Baaki sab |
| `a` | All (user + group + others) | Sab ke liye |
| `+` | Add permission | Permission add karo |
| `-` | Remove permission | Permission hata do |
| `=` | Set exact permission | Exact ye permission rakho |

---

### Example 1: Adding Execute Permission

**Scenario:** You created a script file `script.sh` and want to make it executable.

**Command:**
```bash
chmod +x script.sh
```

**👉 What it does:**
- File me execute (x) permission add ho gayi
- Ab tum `./script.sh` chala sakte ho

**Alternative (more specific):**
```bash
chmod u+x script.sh
```
This adds execute permission only for the owner (user).

---

### Example 2: Removing Write Permission

**Scenario:** You want to protect a file so the group cannot modify it.

**Command:**
```bash
chmod g-w file.txt
```

**👉 What it does:**
- Group se write permission hata di
- Ab group wale log file nahi badal sakte

---

### Example 3: Setting Multiple Permissions

**Command:**
```bash
chmod u+rwx,g+rx,o+r file.txt
```

**What it does:**
- Owner: read + write + execute
- Group: read + execute
- Others: read only

---

## Numeric Permission System

Linux me permissions ko numbers se bhi likhte hain. This is faster and more common.

### Permission Numbers

| Permission | Number | rwx Format |
| :--- | :--- | :--- |
| No permission | `0` | `---` |
| Execute only | `1` | `--x` |
| Write only | `2` | `-w-` |
| Write + Execute | `3` | `-wx` |
| Read only | `4` | `r--` |
| Read + Execute | `5` | `r-x` |
| Read + Write | `6` | `rw-` |
| Read + Write + Execute | `7` | `rwx` |

---

### How Numbers Work

**Formula:** Read (4) + Write (2) + Execute (1) = Total

**Examples:**
- `r = 4`
- `w = 2`
- `x = 1`

**Adding them:**
- `rwx = 4 + 2 + 1 = 7` (full permission)
- `rw- = 4 + 2 + 0 = 6` (read + write, no execute)
- `r-x = 4 + 0 + 1 = 5` (read + execute, no write)
- `r-- = 4 + 0 + 0 = 4` (read only)

---

### Linux Permissions Table

| Number | rwx | Meaning (English) | Meaning (Urdu) |
| :--- | :--- | :--- | :--- |
| `0` | `---` | No permission | Bilkul access nahi |
| `1` | `--x` | Execute only | Sirf chalane ki ijaazat |
| `2` | `-w-` | Write only | Sirf likhne ki ijaazat |
| `3` | `-wx` | Write + Execute | Likho aur chalao |
| `4` | `r--` | Read only | Sirf padhne ki ijaazat |
| `5` | `r-x` | Read + Execute | Padhna aur chalana |
| `6` | `rw-` | Read + Write | Padhna aur likhna |
| `7` | `rwx` | Full access | Padhna, likhna, chalana (sab kuch) |

---

### Common Permission Combinations

| Permission | Meaning (English) | Meaning (Urdu) | Use Case |
| :--- | :--- | :--- | :--- |
| `777` | Everyone can do everything | Sabko full access (Owner, Group, Others) | ⚠️ Dangerous! Avoid this |
| `755` | Owner: full, Others: read+execute | Owner ko sab, others ko sirf dekhna/chalana | Scripts, executable files |
| `644` | Owner: read+write, Others: read only | Owner ko edit karne do, others sirf dekh sakte hain | Text files, documents |
| `600` | Owner: read+write, Others: nothing | Sirf owner dekh/edit kar sakta hai | Private files, passwords |
| `700` | Owner: full, Others: nothing | Sirf owner ko full access | Private scripts |

---

### Example: Using Numeric Permissions

**Scenario 1: Make script executable for owner only**
```bash
chmod 700 script.sh
```

**Breakdown:**
- First `7` = Owner gets `rwx` (read, write, execute)
- Second `0` = Group gets `---` (no permissions)
- Third `0` = Others get `---` (no permissions)

---

**Scenario 2: Common file permissions**
```bash
chmod 644 document.txt
```

**Breakdown:**
- First `6` = Owner gets `rw-` (read, write)
- Second `4` = Group gets `r--` (read only)
- Third `4` = Others get `r--` (read only)

---

**Scenario 3: Give everyone full access (⚠️ Not recommended!)**
```bash
chmod 777 file.txt
```

**Breakdown:**
- First `7` = Owner gets `rwx`
- Second `7` = Group gets `rwx`
- Third `7` = Others get `rwx`

**👉 Warning:** This is dangerous! Everyone can modify or delete the file.

---

## Changing Ownership (chown, chgrp)

### chown - Change Owner

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `chown user file` | Change file owner | File ka owner badal do | `sudo chown ali file.txt` |
| `chown user:group file` | Change owner and group | Owner aur group dono badal do | `sudo chown ali:developers file.txt` |
| `chown -R user folder/` | Recursively change ownership | Folder aur uske andar ki sab files ka owner badlo | `sudo chown -R ali myfolder/` |

---

### chgrp - Change Group

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `chgrp group file` | Change file's group | File ka group badal do | `sudo chgrp developers file.txt` |
| `chgrp -R group folder/` | Recursively change group | Folder aur uske andar ki sab files ka group badlo | `sudo chgrp -R developers project/` |

---

### Example: Changing File Ownership

**Command:**
```bash
sudo chgrp developers file.txt
```

**👉 What it does:**
- File ka group "developers" ho gaya
- Ab "developers" group ke members file access kar sakte hain

---

## User Management Commands

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `whoami` | Show current logged-in user | Bataata hai ke abhi kaun user system me login hai | `whoami` |
| `id` | Show user ID (UID), group ID (GID), and groups | User ka ID number kya hai aur woh kin groups me hai | `id` |
| `sudo adduser <name>` | Create a new user | System me ek naya user "ali" ban gaya | `sudo adduser ali` |
| `sudo passwd <name>` | Set/change user password | User "ali" ka password set/change ho gaya | `sudo passwd ali` |
| `sudo deluser <name>` | Delete a user | User "ali" ko system se hata do | `sudo deluser ali` |
| `su <name>` | Switch to another user | Ab "ali" user ke account me chalay gaye | `su ali` |
| `su -` | Switch to root user | Root (superuser) ban gaye | `su -` |

---

### 1. whoami

**English:** Shows current logged-in user.

**Urdu:** Bataata hai ke abhi kaun user system me login hai.

**Example:**
```bash
whoami
```

**👉 Output:**
```
kali
```

**Use Case:** Quick check to see which user you're currently logged in as.

---

### 2. id

**English:** Shows user ID (UID), group ID (GID), and groups of current user.

**Urdu:** Yeh batata hai ke user ka ID number kya hai aur woh kin groups me hai.

**Example:**
```bash
id
```

**👉 Output:**
```
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

**Breakdown:**
- `uid=1000(kali)` = User ID is 1000, username is "kali"
- `gid=1000(kali)` = Primary group ID is 1000
- `groups=1000(kali),27(sudo)` = User belongs to "kali" and "sudo" groups

**Use Case:** Check user's ID and group memberships.

---

### 3. adduser / useradd

**English:** Create a new user.

**Urdu:** System me ek naya user ban gaya.

**Example:**
```bash
sudo adduser ali
```

**👉 What it does:**
- Creates a new user named "ali"
- Creates home directory `/home/ali`
- Prompts for password and user information
- Sets up default shell

**Alternative:**
```bash
sudo useradd -m -s /bin/bash ali
```
(`-m` creates home directory, `-s` sets shell)

**Use Case:** Add new users to the system.

---

### 4. passwd

**English:** Set or change password of a user.

**Urdu:** User ka password set/change ho gaya.

**Example:**
```bash
sudo passwd ali
```

**👉 What it does:**
- Prompts you to enter new password for user "ali"
- Password is encrypted and stored

**Change your own password:**
```bash
passwd
```

**Use Case:** Set or update user passwords.

---

### 5. deluser / userdel

**English:** Delete a user.

**Urdu:** User ko system se hata do.

**Example:**
```bash
sudo deluser ali
```

**Delete user and their home directory:**
```bash
sudo deluser --remove-home ali
```

**Alternative:**
```bash
sudo userdel -r ali
```
(`-r` removes home directory)

**Use Case:** Remove users from the system.

---

### 6. su (Switch User)

**English:** Switch to another user account.

**Urdu:** Ab "ali" user ke account me chalay gaye.

**Example:**
```bash
su ali
```

**👉 What it does:**
- Switches to user "ali"
- Asks for ali's password
- Your prompt changes to ali's environment

**Switch to root:**
```bash
su -
```
or
```bash
su root
```

**Exit back to original user:**
```bash
exit
```

**Use Case:** Work as different user temporarily.

---

## Group Management

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `groups` | Show groups of current user | Current user kis kis group me belong karta hai | `groups` |
| `groups <user>` | Show groups of specific user | Specific user ke groups dekhna | `groups ali` |
| `sudo groupadd <name>` | Create a new group | Naya group bana do | `sudo groupadd developers` |
| `sudo groupdel <name>` | Delete a group | Group ko delete kar do | `sudo groupdel developers` |
| `sudo usermod -aG <group> <user>` | Add user to a group | User ko group me add karo | `sudo usermod -aG developers ali` |

---

### 1. groups

**English:** Show groups of a user.

**Urdu:** Bataata hai ke user kin kin groups me belong karta hai.

**Example:**
```bash
groups ali
```

**👉 Output:**
```
ali : ali developers sudo
```

**Use Case:** Check which groups a user belongs to.

---

### 2. groupadd

**English:** Create a new group.

**Urdu:** Naya group bana do.

**Example:**
```bash
sudo groupadd developers
```

**Use Case:** Create groups for organizing users with similar permissions.

---

### 3. usermod -aG

**English:** Add user to a group.

**Urdu:** User ko group me add karo.

**Example:**
```bash
sudo usermod -aG developers ali
```

**👉 What it does:**
- Adds user "ali" to "developers" group
- `-a` = append (don't remove from other groups)
- `-G` = supplementary groups

**⚠️ Important:** User needs to log out and log back in for changes to take effect.

**Use Case:** Give users access to shared resources by adding them to groups.

---

## Sudo and Root Access

### What is sudo?

**English:** `sudo` allows regular users to run commands with superuser (root) privileges.

**Urdu:** `sudo` se aap root ki tarah powerful commands chala sakte ho.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `sudo <command>` | Run command as root | Root ki tarah kaam karna (admin powers) | `sudo ls /root` |
| `sudo -i` | Start interactive root shell | Root user ban jao (shell mode) | `sudo -i` |
| `sudo su` | Switch to root user | Root user me switch ho jao | `sudo su` |
| `sudo !!` | Run last command with sudo | Last command ko sudo ke saath phir se chalao | `sudo !!` |

---

### Example: Using sudo

**Scenario:** You want to list files in `/root` directory (requires root access).

**Without sudo (will fail):**
```bash
ls /root
```
**👉 Error:** Permission denied

**With sudo:**
```bash
sudo ls /root
```
**👉 Success:** Shows root directory contents

---

### Who Can Use sudo?

Users in the `sudo` group can use sudo command.

**Check if user is in sudo group:**
```bash
groups username
```

**Add user to sudo group:**
```bash
sudo usermod -aG sudo ali
```

---

## Practical Examples

### Example 1: Creating a Shared Project Folder

**Scenario:** Create a folder for "developers" group where everyone can collaborate.

**Step 1: Create the folder**
```bash
mkdir /home/shared/project
```

**Step 2: Change group ownership**
```bash
sudo chgrp developers /home/shared/project
```

**Step 3: Set permissions**
```bash
chmod 770 /home/shared/project
```

**What it does:**
- Owner: rwx (full access)
- Group: rwx (full access)
- Others: --- (no access)

---

### Example 2: Making a Script Executable

**Scenario:** You wrote a script and want to run it.

**Step 1: Create script**
```bash
echo '#!/bin/bash' > myscript.sh
echo 'echo "Hello World"' >> myscript.sh
```

**Step 2: Check permissions**
```bash
ls -l myscript.sh
```
**Output:** `-rw-r--r--` (not executable)

**Step 3: Add execute permission**
```bash
chmod +x myscript.sh
```

**Step 4: Run it**
```bash
./myscript.sh
```

---

### Example 3: Protecting Sensitive Files

**Scenario:** You have a file with passwords that only you should access.

**Command:**
```bash
chmod 600 passwords.txt
```

**What it does:**
- Owner: rw- (can read and write)
- Group: --- (no access)
- Others: --- (no access)

**Verification:**
```bash
ls -l passwords.txt
```
**Output:** `-rw-------`

---

### Example 4: Adding User to Multiple Groups

**Scenario:** Add user "ali" to both "developers" and "docker" groups.

**Commands:**
```bash
sudo usermod -aG developers ali
sudo usermod -aG docker ali
```

**Verify:**
```bash
groups ali
```
**Output:** `ali : ali developers docker`

---

## Common Use Cases

### Use Case 1: Web Server File Permissions

**Scenario:** Apache/Nginx web server files.

**Directory permissions:**
```bash
sudo chmod 755 /var/www/html
```

**File permissions:**
```bash
sudo chmod 644 /var/www/html/index.html
```

**Why?**
- Web server needs to read files (but not execute)
- Owner can edit files
- Public can view website

---

### Use Case 2: SSH Key Permissions

**Scenario:** Secure SSH private key.

**Command:**
```bash
chmod 600 ~/.ssh/id_rsa
```

**Why?**
- Only owner should access private key
- SSH will refuse to work if permissions are too open

---

### Use Case 3: Executable Scripts in /usr/local/bin

**Scenario:** Make script available system-wide.

**Commands:**
```bash
sudo cp myscript.sh /usr/local/bin/myscript
sudo chmod 755 /usr/local/bin/myscript
```

**Why?**
- Everyone can execute the script
- Only root can modify it

---

## Best Practices

### 1. Permission Best Practices

**DO:**
- Use `755` for directories and executable files
- Use `644` for regular files
- Use `600` for sensitive files (passwords, keys)
- Set appropriate ownership with `chown`

**DON'T:**
- Never use `777` (everyone can do everything)
- Don't give write permission to others unnecessarily
- Don't run everything as root
- Don't ignore permission warnings

---

### 2. User Management Best Practices

**DO:**
- Create separate users for different people
- Use groups for shared permissions
- Use strong passwords
- Remove users who no longer need access

**DON'T:**
- Don't share root password
- Don't give sudo access to everyone
- Don't use the same password for multiple users
- Don't leave unused accounts active

---

### 3. Sudo Best Practices

**DO:**
- Use `sudo` for administrative tasks
- Check what command does before running with sudo
- Use `sudo -l` to see what you're allowed to run

**DON'T:**
- Don't stay logged in as root
- Don't give sudo access without careful consideration
- Don't run untrusted scripts with sudo

---

### 4. Security Tips

**Check file permissions regularly:**
```bash
find /home -type f -perm 777
```

**Find files owned by specific user:**
```bash
find / -user ali 2>/dev/null
```

**Find files with SUID bit (security risk):**
```bash
find / -perm -4000 2>/dev/null
```

---

## Conclusion

Understanding Linux users and permissions is fundamental to system security and proper file management. These concepts ensure that users can only access what they're authorized to, protecting both system files and user data.

**Quick Reference Summary:**

| Task | Command | Use Case |
| :--- | :--- | :--- |
| View permissions | `ls -l` | Check file permissions |
| Change permissions | `chmod 755 file` | Make file executable |
| Change owner | `sudo chown user file` | Transfer ownership |
| Change group | `sudo chgrp group file` | Change file's group |
| Add execute | `chmod +x script.sh` | Make script runnable |
| Remove write | `chmod -w file` | Protect from changes |
| Show current user | `whoami` | Identity check |
| Show user details | `id` | See UID, GID, groups |
| Add user | `sudo adduser ali` | Create new user |
| Change password | `sudo passwd ali` | Update password |
| Switch user | `su ali` | Login as different user |
| Run as root | `sudo command` | Admin privileges |
| Show groups | `groups ali` | Check group membership |
| Add to group | `sudo usermod -aG group user` | Grant group access |

---

**Key Takeaways:**

1. **Permissions** use `rwx` (read, write, execute) for three categories
2. **Categories** are Owner, Group, and Others
3. **chmod** changes permissions (use `755`, `644`, `600` commonly)
4. **Numeric system** is faster: `7=rwx`, `6=rw-`, `5=r-x`, `4=r--`
5. **chown/chgrp** change ownership and group
6. **User management** commands: `adduser`, `passwd`, `su`, `sudo`
7. **Groups** organize users with shared permissions
8. **sudo** provides temporary root privileges
9. **Never use 777** unless absolutely necessary
10. **Security first** - always use minimum required permissions

---

**Next Steps:**

1. Practice viewing permissions with `ls -l`
2. Experiment with `chmod` on test files
3. Create a test user and practice user management
4. Set up a shared folder with group permissions
5. Learn about special permissions (SUID, SGID, Sticky Bit)
6. Study `/etc/passwd` and `/etc/group` files
7. Practice using `sudo` responsibly

For more detailed information, consult the manual pages:
*   `man chmod`
*   `man chown`
*   `man useradd`
*   `man sudo`
*   `man passwd`

---

**Remember:** Proper permissions are the foundation of Linux security. Always apply the principle of least privilege - give users only the permissions they actually need!