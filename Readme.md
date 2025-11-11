d# 🐧 Linux Command Line Essentials

A comprehensive guide to essential Linux commands for beginners and intermediate users. This repository contains 8 detailed modules covering everything from basic file operations to advanced networking and system administration.

## 📚 Course Structure

### 🎯 Beginner Level

#### **Module 1: File & Directory Management**
Learn the fundamental commands for navigating and managing files in Linux.

**What You'll Learn:**
- Navigate directories (`pwd`, `cd`, `ls`)
- Create and remove directories (`mkdir`, `rmdir`)
- Manage files (`rm`, `cp`, `mv`)
- Understanding file paths and permissions basics

**Key Commands:** `pwd`, `ls`, `cd`, `mkdir`, `rmdir`, `rm`, `cp`, `mv`

📖 [Go to Module 1](./modules/01_file_directory_management.md)

---

#### **Module 2: File Compression & Archiving**
Master file compression and archiving techniques.

**What You'll Learn:**
- Create archives with `tar`
- Compress files with `gzip`
- Work with ZIP files
- Combine archiving and compression

**Key Commands:** `tar`, `gzip`, `gunzip`, `zip`, `unzip`

📖 [Go to Module 2](./modules/02_file_compression_archiving.md)

---

#### **Module 3: File Viewing Commands**
Learn to view, search, and compare file contents.

**What You'll Learn:**
- Display file contents (`cat`, `less`, `more`)
- View partial content (`head`, `tail`)
- Search files with `grep`
- Count words and lines with `wc`
- Compare files with `diff`)

**Key Commands:** `cat`, `less`, `more`, `head`, `tail`, `grep`, `wc`, `diff`

📖 [Go to Module 3](./modules/03_file_viewing.md)

---

### 🚀 Intermediate Level

#### **Module 4: System & Process Essentials**
Understand and manage system processes and resources.

**What You'll Learn:**
- View running processes (`ps`, `top`, `htop`)
- Terminate processes (`kill`, `pkill`)
- Monitor system resources (`uptime`, `free`, `df`)
- Manage background jobs

**Key Commands:** `ps`, `top`, `htop`, `kill`, `uptime`, `free`, `df`, `du`

📖 [Go to Module 4](./modules/04_system_process_essentials.md)

---

#### **Module 5: Linux Users & Permissions**
Master user management and file permissions.

**What You'll Learn:**
- Understanding permission system (rwx)
- Change permissions (`chmod`)
- Change ownership (`chown`, `chgrp`)
- User management (`adduser`, `passwd`, `su`)
- Group management

**Key Commands:** `chmod`, `chown`, `chgrp`, `adduser`, `passwd`, `su`, `sudo`

📖 [Go to Module 5](./modules/05_users_permissions.md)

---

### 🔧 System Administration

#### **Module 6: /var/log Management**
Learn to work with system logs for troubleshooting.

**What You'll Learn:**
- Understanding log file structure
- Important log files (auth.log, syslog, kern.log)
- Real-time log monitoring
- Log rotation with `logrotate`

**Key Files:** `/var/log/auth.log`, `/var/log/syslog`, `/var/log/kern.log`

📖 [Go to Module 6](./modules/06_var_log_management.md)

---

#### **Module 7: APT Package Management**
Master Debian/Ubuntu package management.

**What You'll Learn:**
- Install and remove packages
- Update system packages
- Search for packages
- Manage repositories
- Troubleshoot package issues

**Key Commands:** `apt update`, `apt upgrade`, `apt install`, `apt remove`, `apt search`

📖 [Go to Module 7](./modules/07_apt_package_management.md)

---

#### **Module 8: Networking Essentials**
Essential networking commands for diagnostics and management.

**What You'll Learn:**
- Test connectivity (`ping`, `traceroute`)
- DNS queries (`nslookup`, `dig`)
- Network configuration (`ifconfig`, `ip`)
- Monitor connections (`netstat`)
- Transfer files (`scp`, `wget`, `curl`)

**Key Commands:** `ping`, `traceroute`, `nslookup`, `dig`, `netstat`, `scp`, `curl`

📖 [Go to Module 8](./modules/08_networking_essentials.md)

---

## 🎓 Learning Path

### Recommended Order for Beginners

```
START HERE
    ↓
Module 1: File & Directory Management
    ↓
Module 2: File Compression & Archiving
    ↓
Module 3: File Viewing Commands
    ↓
Module 4: System & Process Essentials
    ↓
Module 5: Users & Permissions
    ↓
Module 6: /var/log Management
    ↓
Module 7: APT Package Management
    ↓
Module 8: Networking Essentials
    ↓
CONGRATULATIONS! 🎉
```

### Quick Start Path (Essential Commands Only)

If you're in a hurry, focus on these modules first:
1. **Module 1** - Basic navigation
2. **Module 3** - View files
3. **Module 4** - Manage processes

---

## 📋 Quick Reference

### Most Used Commands by Category

#### File Operations
```bash
ls -lah          # List files with details
cd /path         # Change directory
cp file1 file2   # Copy files
mv old new       # Move/rename
rm file          # Remove file
```

#### File Viewing
```bash
cat file.txt     # Display entire file
less file.txt    # Scrollable view
tail -f log.txt  # Monitor logs
grep "text" file # Search in file
```

#### System Monitoring
```bash
top              # Process monitor
df -h            # Disk space
free -h          # Memory usage
ps aux           # Running processes
```

#### Package Management
```bash
sudo apt update          # Update package list
sudo apt upgrade         # Upgrade packages
sudo apt install pkg     # Install package
sudo apt remove pkg      # Remove package
```

#### Networking
```bash
ping 8.8.8.8            # Test connectivity
ifconfig / ip addr      # Network config
netstat -tulnp          # Open ports
curl https://site.com   # Test web server
```

---

## 🎯 Skills You'll Master

After completing all modules, you will be able to:

✅ Navigate Linux filesystem confidently  
✅ Manage files and directories efficiently  
✅ View and search file contents  
✅ Monitor and control system processes  
✅ Set proper file permissions  
✅ Analyze system logs  
✅ Install and manage software packages  
✅ Diagnose network issues  
✅ Troubleshoot common Linux problems  

---

## 🛠️ Prerequisites

- Linux system (Ubuntu, Debian, Kali, or any Linux distribution)
- Terminal access
- Basic computer literacy
- Willingness to learn!

---

## 💡 How to Use This Repository

### For Complete Beginners
1. Start with Module 1
2. Follow the recommended learning path
3. Practice each command as you read
4. Complete the examples in each module
5. Don't skip the practical examples!

### For Quick Reference
- Use the Quick Reference section above
- Jump to specific modules as needed
- Search for specific commands using Ctrl+F

### For Hands-On Learning
- Create test files and directories
- Try all examples yourself
- Break things in a safe environment (virtual machine recommended)
- Experiment with different flags and options

---

## 📖 Module Details

| Module | Level | Time to Complete | Key Focus |
|--------|-------|------------------|-----------|
| Module 1 | Beginner | 1-2 hours | File navigation |
| Module 2 | Beginner | 1 hour | Archiving |
| Module 3 | Beginner | 2-3 hours | File viewing |
| Module 4 | Intermediate | 2-3 hours | Process management |
| Module 5 | Intermediate | 2-3 hours | Permissions |
| Module 6 | Advanced | 1-2 hours | System logs |
| Module 7 | Advanced | 2 hours | Package management |
| Module 8 | Advanced | 3-4 hours | Networking |

**Total Learning Time:** ~15-20 hours

---

## 🔥 Pro Tips

1. **Practice in a VM:** Use VirtualBox or VMware for safe experimentation
2. **Use Tab Completion:** Press Tab to auto-complete commands and filenames
3. **Read Man Pages:** Type `man <command>` for detailed documentation
4. **Keep Notes:** Document commands you use frequently
5. **Build Aliases:** Create shortcuts for common commands in `~/.bashrc`

---

## 🤝 Contributing

Contributions are welcome! If you find errors or want to add content:
1. Fork this repository
2. Create a new branch
3. Make your changes
4. Submit a pull request

---

## 📜 License

This educational content is free to use and distribute.

---

## 👤 Author

**Abdul Wahid**  
GitHub: [@abdul-wahid022](https://github.com/abdul-wahid022)

---

## 🌟 Support

If you find this helpful:
- ⭐ Star this repository
- 🔄 Share with others
- 📢 Provide feedback

---

## 📞 Get Help

Stuck? Here's how to get help:
1. Re-read the module carefully
2. Check the practical examples
3. Consult `man <command>` for details
4. Search online for specific error messages
5. Ask in Linux forums or communities

---

**Happy Learning! 🚀**

*Remember: The best way to learn Linux is by doing. Don't just read - practice every command!*

---

## 📚 Additional Resources

- [Linux Journey](https://linuxjourney.com/) - Interactive Linux tutorials
- [ExplainShell](https://explainshell.com/) - Break down command syntax
- [TLDRPages](https://tldr.sh/) - Simplified man pages
- [Linux Command Library](https://linuxcommandlibrary.com/) - Searchable command database

---

**Last Updated:** November 2024  
**Version:** 1.0
