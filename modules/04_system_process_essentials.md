# Module 4: System & Process Essentials

System and process management commands are fundamental skills for anyone working with Linux. These commands allow you to monitor system resources, manage running processes, check disk usage, and control background jobs. Whether you're troubleshooting performance issues, killing frozen applications, or monitoring system health, mastering these commands is essential for effective system administration.

### Table of Contents

1.  [What are System & Process Management Commands?](#what-are-system--process-management-commands)
2.  [Understanding Processes](#understanding-processes)
3.  [Process Viewing Commands](#process-viewing-commands)
4.  [Process Management Commands](#process-management-commands)
5.  [System Monitoring Commands](#system-monitoring-commands)
6.  [Disk Space Management](#disk-space-management)
7.  [Memory Management](#memory-management)
8.  [Background Job Control](#background-job-control)
9.  [Practical Examples](#practical-examples)
10. [Common Use Cases](#common-use-cases)
11. [Best Practices](#best-practices)
12. [Conclusion](#conclusion)

---

## What are System & Process Management Commands?

System and process management commands are utilities that allow you to interact with and control the operating system's processes, monitor resource usage, and manage system performance. These commands are particularly useful for:
- Monitoring running applications and services
- Identifying resource-intensive processes
- Terminating unresponsive applications
- Checking system uptime and load
- Managing disk space and memory usage
- Controlling background jobs

**Key Benefits:**
- **Real-Time Monitoring:** View system activity as it happens
- **Resource Management:** Identify and control resource-hungry processes
- **Troubleshooting:** Diagnose performance issues quickly
- **System Health:** Keep track of disk space, memory, and CPU usage

---

## Understanding Processes

### What is a Process?

**English:** A process is a running instance of a program. Every application you run creates at least one process.

**Urdu:** Process ek running program hai. Jab bhi aap koi software chalate ho, wo ek process ban jata hai.

---

### What is PID?

**PID (Process ID):** A unique number assigned to each running process by the operating system.

**Urdu:** Har process ko ek unique number milta hai, jisse PID kehte hain.

**Example:** If Firefox is running with PID 2314, you can use this number to monitor or terminate it.

---

## Process Viewing Commands

The following table lists the most commonly used process viewing commands:

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `ps` | Show current user's processes | Current user ke processes dikhata hai | `ps` |
| `ps aux` | Show all running processes (detailed) | Sabhi running processes detail me dikhata hai | `ps aux` |
| `ps aux \| grep <name>` | Find specific process | Kisi specific process ko dhoondhta hai | `ps aux \| grep firefox` |
| `top` | Real-time process viewer | Processes ko real-time me dikhata hai | `top` |
| `htop` | Enhanced interactive process viewer | Top ka behtar version (colors aur easy navigation) | `htop` |
| `pgrep <name>` | Find process ID by name | Process ka PID naam se dhoondhta hai | `pgrep firefox` |

---

### 1. ps (Process Status)

**English:** Shows running processes.

**Urdu:** Kaunse processes chal rahe hain aur unka Process ID (PID) kya hai.

**Example:**
```bash
ps
```

**👉 Output:**
```
PID   TTY      TIME CMD
1234  pts/0    00:00:00 bash
```

**Explanation:**
- `PID` = Process ID (Unique number)
- `TTY` = Terminal type
- `TIME` = CPU time used
- `CMD` = Command name

**Use Case:** Quick check of your own running processes.

---

### 2. ps aux

**English:** Shows all processes running on the system with detailed information.

**Urdu:** Sabhi processes detail me dikhata hai - CPU, memory usage, owner, everything.

**Example:**
```bash
ps aux
```

**👉 Output:**
```
USER   PID  %CPU %MEM    VSZ   RSS TTY   STAT START   TIME COMMAND
kali   2314  2.5  4.2 123456 98765 ?     Sl   10:20   0:15 firefox
root   1234  0.1  0.5  12345  5678 ?     Ss   09:00   0:02 systemd
```

**Important Columns:**
- `USER` = Who owns the process
- `PID` = Process ID
- `%CPU` = CPU usage percentage
- `%MEM` = Memory usage percentage
- `COMMAND` = Process name

**Use Case:** Complete overview of all system processes - perfect for troubleshooting.

---

### 3. Finding Specific Processes

**English:** Use `ps aux` with `grep` to find specific processes.

**Urdu:** Kisi specific process ko dhoondhne ke liye `ps aux` aur `grep` ko milao.

**Example:**
```bash
ps aux | grep firefox
```

**👉 Output:**
```
kali    2314  2.5  4.2 123456 98765 ?   Sl  10:20   0:15 firefox
```

**Alternative (cleaner):**
```bash
pgrep firefox
```

**👉 Output:**
```
2314
```

---

### 4. top

**English:** Real-time view of system processes and resource usage. Like Windows Task Manager but in terminal.

**Urdu:** Jo Windows me Task Manager hota hai, wahi kaam yeh karta hai terminal me.

**Example:**
```bash
top
```

**Key Navigation:**
- Press `q` → Quit
- Press `k` → Kill a process (enter PID)
- Press `M` → Sort by memory usage
- Press `P` → Sort by CPU usage
- Press `1` → Show individual CPU cores

**Use Case:** Real-time monitoring of system performance.

---

### 5. htop

**English:** Enhanced version of `top` with colors, mouse support, and easier navigation.

**Urdu:** Colorful aur easy interface me processes dekh sakte ho.

**Example:**
```bash
htop
```

**👉 Note:** If not installed:
```bash
sudo apt install htop -y
```

**Key Features:**
- Color-coded display
- Mouse support
- Easy process kill with `F9`
- Search with `F3`
- Tree view with `F5`

**Use Case:** User-friendly real-time system monitoring.

---

## Process Management Commands

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `kill <PID>` | Gracefully stop a process | Process ko normally band karta hai | `kill 2314` |
| `kill -9 <PID>` | Force kill a process | Process ko forcefully turant band karta hai | `kill -9 2314` |
| `killall <name>` | Kill all processes with that name | Same naam ke saare processes ko band karta hai | `killall firefox` |
| `pkill <name>` | Kill processes by name | Process name se directly band karta hai | `pkill firefox` |

---

### 1. kill (Terminate Process)

**English:** Terminates a process using its PID.

**Urdu:** PID 1234 wala process band kar do.

**Example:**
```bash
kill 2314
```

**What It Does:**
- Sends graceful termination request
- Process gets time to clean up
- Saves data if possible

**Use Case:** Normal way to stop a process.

---

### 2. kill -9 (Force Kill)

**English:** Forcefully terminates a process immediately.

**Urdu:** Agar normal kill na ho to force karne ke liye.

**Example:**
```bash
kill -9 2314
```

**What It Does:**
- Immediate termination
- Process cannot ignore this
- No cleanup time

**⚠️ Warning:** Use only when normal `kill` doesn't work.

**Use Case:** Frozen applications that don't respond.

---

### 3. pkill (Kill by Name)

**English:** Kills processes by name without finding PID first.

**Urdu:** Process ko naam se kill karta hai.

**Example:**
```bash
pkill firefox
```

**Force kill version:**
```bash
pkill -9 firefox
```

**Use Case:** Quick termination without finding PID.

---

## System Monitoring Commands

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `uptime` | Show system uptime and load | System kitne time se chal raha hai aur load kya hai | `uptime` |
| `uptime -p` | Pretty uptime format | Simple aur readable format me uptime | `uptime -p` |
| `w` | Who is logged in | Kaun kaun login hai | `w` |
| `whoami` | Current username | Current user ka naam | `whoami` |
| `uname -a` | System information | System ki complete information | `uname -a` |

---

### 1. uptime

**English:** Shows how long the system has been running and the load average.

**Urdu:** System kab se chal raha hai aur kitna busy hai.

**Example:**
```bash
uptime
```

**👉 Output:**
```
10:30:45 up 1 day,  2:15,  2 users,  load average: 0.12, 0.08, 0.05
```

**Breaking It Down:**
- `10:30:45` = Current time
- `up 1 day, 2:15` = System uptime
- `2 users` = Currently logged in users
- `load average: 0.12, 0.08, 0.05` = Load for last 1, 5, 15 minutes

**Use Case:** Quick system health check.

---

### 2. uptime -p

**English:** Shows uptime in readable format.

**Urdu:** Sirf readable uptime dikhata hai.

**Example:**
```bash
uptime -p
```

**👉 Output:**
```
up 3 hours, 20 minutes
```

**Use Case:** Human-friendly uptime display.

---

## Disk Space Management

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `df -h` | Show disk space usage | Kitni storage use hui aur kitni free hai | `df -h` |
| `du -h <path>` | Show folder/file size | Folder ya file ka size | `du -h myfolder/` |
| `du -sh <path>` | Summary of folder size | Sirf total size summary | `du -sh myfolder/` |
| `du -sh *` | Size of all items in directory | Current folder me sab ki size | `du -sh *` |

---

### 1. df -h (Disk Free)

**English:** Shows disk space usage for all mounted filesystems.

**Urdu:** Kitni storage use hui aur kitni free hai, human readable format me.

**Example:**
```bash
df -h
```

**👉 Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
/dev/sda2       100G   45G   50G  48% /home
```

**Important Columns:**
- `Size` = Total size
- `Used` = Used space
- `Avail` = Available space
- `Use%` = Usage percentage

**📌 Note:** `-h` means "human readable" (shows GB/MB instead of bytes).

**Use Case:** Check if disk is full, identify which partition needs cleanup.

---

### 2. du -h (Disk Usage)

**English:** Shows the size of directories and files.

**Urdu:** Folder ya file ka size kitna hai.

**Example:**
```bash
du -h myfolder/
```

**Use Case:** Find which folders are taking up space.

---

### 3. du -sh (Summary)

**English:** Shows only total size of a directory.

**Urdu:** Sirf total size summary.

**Example:**
```bash
du -sh myfolder/
```

**👉 Output:**
```
12M     myfolder/
```

**Flags:**
- `-s` = Summary (total only)
- `-h` = Human readable

**Use Case:** Quick size check without details.

---

## Memory Management

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `free -h` | Show RAM & swap usage | RAM aur swap usage human readable format me | `free -h` |
| `free -s 5` | Update every 5 seconds | Har 5 second me update karo (live monitoring) | `free -s 5` |

---

### 1. free -h

**English:** Shows RAM and swap memory usage.

**Urdu:** Kitni RAM use ho rahi hai aur kitni free hai.

**Example:**
```bash
free -h
```

**👉 Output:**
```
              total        used        free      shared  buff/cache   available
Mem:          7.7Gi       3.4Gi       1.2Gi       234Mi       3.1Gi       4.1Gi
Swap:         2.0Gi          0B       2.0Gi
```

**Important Columns:**
- `total` = Total RAM installed
- `used` = Currently in use
- `free` = Completely unused
- `available` = Available for new apps (most important!)

**📌 Note:** Linux uses free RAM for caching - this is normal and good for performance.

**Use Case:** Check if system is running out of memory.

---

### 2. free with Live Updates

**Example:**
```bash
free -h -s 5
```

**What It Does:**
- `-s 5` = Update every 5 seconds
- Shows memory usage in real-time

**Stop with:** `Ctrl+C`

**Use Case:** Monitor memory during heavy operations.

---

## Background Job Control

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `jobs` | List background jobs | Background me chal rahe jobs dekhna | `jobs` |
| `fg %1` | Bring job to foreground | Background job ko foreground me lao | `fg %1` |
| `bg %1` | Continue job in background | Background me chalao | `bg %1` |
| `command &` | Run in background | Command ko background me start karo | `firefox &` |
| `Ctrl+Z` | Suspend current process | Current process ko rok do | `Ctrl+Z` |

---

### 1. jobs

**English:** Shows background jobs running in current terminal.

**Urdu:** Background me chal rahe jobs dekhna.

**Example:**
```bash
jobs
```

**👉 Output:**
```
[1]+  Running                 firefox &
[2]-  Stopped                 vim document.txt
```

**Use Case:** Check what's running in background.

---

### 2. fg (Foreground)

**English:** Brings a background job to foreground.

**Urdu:** Pehla background job wapas screen pe lao.

**Example:**
```bash
fg %1
```

**Use Case:** Resume working with a background application.

---

### 3. bg (Background)

**English:** Continues a stopped job in background.

**Urdu:** Background me chalao.

**Example:**

**Step 1:** Run a command
```bash
sleep 100
```

**Step 2:** Press `Ctrl+Z` to suspend
```
^Z
[1]+  Stopped                 sleep 100
```

**Step 3:** Continue in background
```bash
bg %1
```

**Use Case:** Continue a suspended process without blocking terminal.

---

### 4. Running in Background

**English:** Add `&` at the end to run in background.

**Urdu:** Agar koi program background me chalana hai to last me & lagao.

**Example:**
```bash
firefox &
```

**What It Does:**
- Starts Firefox
- Returns control to terminal
- Process runs in background

**Use Case:** Start GUI apps without blocking terminal.

---

## Practical Examples

### Example 1: Finding and Killing Frozen Firefox

**Step 1: Find PID**
```bash
ps aux | grep firefox
```

**Output:**
```
kali    2314  25.5  8.2 523456 298765 ?   Rl  10:20  15:15 firefox
```

**Step 2: Try graceful kill**
```bash
kill 2314
```

**Step 3: If stuck, force kill**
```bash
kill -9 2314
```

**Quick Alternative:**
```bash
pkill -9 firefox
```

---

### Example 2: Monitoring System Resources

**Check disk space:**
```bash
df -h
```

**Check memory:**
```bash
free -h
```

**Check running processes:**
```bash
htop
```

**Check system uptime:**
```bash
uptime
```

---

### Example 3: Finding Large Files

**Find top 10 largest files:**
```bash
du -ah . | sort -rh | head -n 10
```

**What It Does:**
- `du -ah .` = Show all file sizes
- `sort -rh` = Sort by size (largest first)
- `head -n 10` = Show top 10

**Use Case:** Free up disk space by finding large files.

---

### Example 4: Background Task Management

**Start a long process in background:**
```bash
tar -czf backup.tar.gz /home/user/ &
```

**Check if it's running:**
```bash
jobs
```

**Monitor its progress:**
```bash
ps aux | grep tar
```

---

## Common Use Cases

### Use Case 1: Server Health Check

**Quick commands to run:**
```bash
uptime              # Check uptime and load
df -h               # Check disk space
free -h             # Check memory
ps aux | head -20   # Check top processes
```

---

### Use Case 2: Killing Multiple Processes

**Kill all Chrome processes:**
```bash
pkill -9 chrome
```

**Kill all Python scripts:**
```bash
pkill -9 python3
```

---

### Use Case 3: Monitoring a Specific Process

**Find process:**
```bash
ps aux | grep myapp
```

**Monitor its CPU/Memory in real-time:**
```bash
top -p <PID>
```

---

## Best Practices

### 1. Process Management

**DO:**
- Always try `kill` before `kill -9`
- Use `htop` for easier visualization
- Monitor system load regularly
- Check disk space frequently

**DON'T:**
- Don't kill system processes (PID 1)
- Don't use `kill -9` as first option
- Don't ignore high load warnings
- Don't let disk fill up to 100%

---

### 2. Regular Monitoring

**Daily checks:**
```bash
# Disk space
df -h | grep -E '^/dev/'

# Memory usage
free -h

# System load
uptime
```

---

### 3. Resource Management

**For heavy tasks:**
- Run in background with `&`
- Use `nice` to lower priority:
  ```bash
  nice -n 10 heavy_task.sh
  ```

**For critical tasks:**
- Monitor with `top` or `htop`
- Keep an eye on system load

---

## Conclusion

System and process management commands are essential for maintaining a healthy Linux system. These tools help you monitor resources, control running applications, and troubleshoot issues efficiently.

**Quick Reference Summary:**

| Task | Command | Use Case |
| :--- | :--- | :--- |
| View processes | `ps aux` | See all running processes |
| Find specific process | `ps aux \| grep firefox` | Find process by name |
| Real-time monitor | `htop` | Interactive process viewer |
| Kill process | `kill <PID>` | Stop a process |
| Force kill | `kill -9 <PID>` | Force stop frozen process |
| Check disk space | `df -h` | See storage usage |
| Check folder size | `du -sh folder/` | See directory size |
| Check memory | `free -h` | See RAM usage |
| System uptime | `uptime` | See how long system is running |
| Background jobs | `jobs` | List background processes |
| Run in background | `command &` | Start process in background |

---

**Key Takeaways:**

1. **ps** shows running processes - use `ps aux` for details
2. **top/htop** provide real-time monitoring - htop is more user-friendly
3. **kill** terminates processes - use `-9` only when needed
4. **df** shows disk usage - monitor to prevent full disks
5. **free** shows memory usage - check when system slows down
6. **uptime** shows system health - watch load averages
7. **jobs** manages background tasks - use `&` to run in background
8. **Practice regularly** - these commands become second nature

---

**Next Steps:**

1. Practice these commands on test system
2. Create your own monitoring script
3. Learn about system logs (`/var/log/`)
4. Explore advanced tools: `sar`, `vmstat`, `iostat`
5. Set up regular monitoring routine

For more detailed information, consult the manual pages:
*   `man ps`
*   `man top`
*   `man kill`
*   `man df`
*   `man free`

---

**Remember:** These commands are essential for system administration. The more you practice, the more efficient you'll become at managing your Linux system!