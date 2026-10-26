# Module 4: System & Processes

**Objective:** See what the system is doing, control running programs, check disk and memory, and manage services and scheduled jobs.

| Section | Topic |
| ------- | ----- |
| [1](#1-processes-and-pids) | Processes and PIDs |
| [2](#2-viewing-processes) | `ps` `top` `htop` `pgrep` |
| [3](#3-stopping-processes) | `kill` `killall` `pkill`, signals |
| [4](#4-background-jobs) | Background jobs |
| [5](#5-priority) | `nice` `renice` |
| [6](#6-system-information) | `uptime` `uname` `lscpu` `lsblk` |
| [7](#7-disk-and-memory) | `df` `du` `free` |
| [8](#8-services-with-systemctl) | `systemctl` |
| [9](#9-scheduling-with-cron) | `cron` |
| [10](#practice) | Practice, common mistakes |

---

## 1. Processes and PIDs

A **process** is a running instance of a program. Every process has a unique **PID** (process ID) and a **PPID** (the PID of its parent). PID 1 is the init system (`systemd`) and must never be killed.

| State | Meaning |
| ----- | ------- |
| `R` | Running or ready to run |
| `S` | Sleeping, waiting for an event |
| `D` | Uninterruptible sleep, usually waiting for disk or network I/O |
| `T` | Stopped (for example with `Ctrl+Z`) |
| `Z` | Zombie: finished, but the parent has not collected it |

---

## 2. Viewing processes

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `ps` | Processes in the current terminal | `ps` |
| `ps aux` | All processes with user, CPU and memory | `ps aux` |
| `ps -ef` | All processes with PPID | `ps -ef` |
| `ps aux --sort=-%mem \| head` | Top memory users | `ps aux --sort=-%mem \| head` |
| `pgrep <name>` | PID of a process by name | `pgrep firefox` |
| `pstree` | Process tree | `pstree` |
| `top` | Live process viewer | `top` |
| `htop` | Friendlier live viewer (`sudo apt install htop`) | `htop` |

```bash
ps aux
```
```
USER   PID  %CPU %MEM    VSZ   RSS TTY   STAT START   TIME COMMAND
kali  2314   2.5  4.2 123456 98765 ?     Sl   10:20   0:15 firefox
root     1   0.1  0.5  12345  5678 ?     Ss   09:00   0:02 systemd
```

| Column | Meaning |
| ------ | ------- |
| `USER` | Owner of the process |
| `PID` | Process ID |
| `%CPU`, `%MEM` | CPU and memory share |
| `STAT` | State (see table above) |
| `COMMAND` | The command that started it |

**Keys in `top`**

| Key | Action |
| --- | ------ |
| `q` | Quit |
| `P` / `M` | Sort by CPU / memory |
| `k` | Kill a process (asks for the PID) |
| `1` | Show each CPU core |

**Keys in `htop`:** `F3` search, `F5` tree view, `F9` kill, `F10` quit.

---

## 3. Stopping processes

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `kill <PID>` | Ask the process to terminate (SIGTERM) | `kill 2314` |
| `kill -9 <PID>` | Force it to stop (SIGKILL), no cleanup | `kill -9 2314` |
| `killall <name>` | Signal every process with that name | `killall firefox` |
| `pkill <name>` | Signal processes matching a name pattern | `pkill firefox` |
| `kill -l` | List all signals | `kill -l` |

| Signal | Number | Meaning |
| ------ | ------ | ------- |
| `SIGHUP` | 1 | Hang up, many daemons reload their config |
| `SIGINT` | 2 | Interrupt (`Ctrl+C`) |
| `SIGKILL` | 9 | Kill immediately, cannot be caught or ignored |
| `SIGTERM` | 15 | Polite request to terminate (default) |
| `SIGSTOP` | 19 | Pause the process (cannot be caught) |
| `SIGCONT` | 18 | Resume a paused process |

Always try `kill` before `kill -9`. A process killed with `-9` gets no chance to save data or remove temporary files. You need `sudo` to signal processes you do not own.

**Example: stop a frozen program**

```bash
ps aux | grep firefox
kill 2314          # try the polite way first
kill -9 2314       # only if it is still running
```

---

## 4. Background jobs

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `command &` | Start in the background | `sleep 100 &` |
| `Ctrl+Z` | Suspend the foreground job | |
| `jobs` | List jobs of this terminal | `jobs` |
| `bg %1` | Resume job 1 in the background | `bg %1` |
| `fg %1` | Bring job 1 to the foreground | `fg %1` |
| `nohup <cmd> &` | Keep running after the terminal closes | `nohup ./script.sh &` |

```bash
sleep 100
# press Ctrl+Z
```
```
[1]+  Stopped                 sleep 100
```
```bash
bg %1
jobs
```

---

## 5. Priority

`nice` sets the priority of a new process (-20 highest to 19 lowest, default 0). `renice` changes a running one.

```bash
nice -n 10 ./heavy_task.sh
sudo renice -n 5 -p 2314
```

---

## 6. System information

| Command | What it shows |
| ------- | ------------- |
| `uptime` | Time since boot, users, load average |
| `uptime -p` | Uptime in readable form |
| `uname -a` | Kernel and architecture |
| `hostnamectl` | Hostname, OS and kernel |
| `whoami` | Current user |
| `w` | Who is logged in and what they run |
| `lscpu` | CPU details |
| `lsblk` | Disks and partitions |
| `vmstat 2 5` | CPU, memory and I/O every 2 seconds, 5 times |
| `watch -n 2 <cmd>` | Re-run a command every 2 seconds |

```bash
uptime
```
```
10:30:45 up 1 day,  2:15,  2 users,  load average: 0.12, 0.08, 0.05
```

The three load averages are for the last 1, 5 and 15 minutes. As a rule of thumb, a load that stays above the number of CPU cores means the system is busy.

---

## 7. Disk and memory

| Command | What it does |
| ------- | ------------ |
| `df -h` | Disk space per filesystem |
| `du -sh <path>` | Total size of a directory |
| `du -sh *` | Size of everything in the current directory |
| `du -ah . \| sort -rh \| head -n 10` | Ten largest files and directories |
| `free -h` | RAM and swap usage |
| `free -h -s 5` | Same, refreshed every 5 seconds |

```bash
df -h
```
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
```

```bash
free -h
```
```
              total        used        free      shared  buff/cache   available
Mem:          7.7Gi       3.4Gi       1.2Gi       234Mi       3.1Gi       4.1Gi
Swap:         2.0Gi          0B       2.0Gi
```

Look at the `available` column, not `free`. Linux uses spare RAM as cache, which is normal and is released when programs need it.

---

## 8. Services with systemctl

Most background services (daemons) are managed by `systemd`.

| Command | What it does |
| ------- | ------------ |
| `systemctl status ssh` | Show whether a service is running |
| `sudo systemctl start ssh` | Start it now |
| `sudo systemctl stop ssh` | Stop it now |
| `sudo systemctl restart ssh` | Stop and start |
| `sudo systemctl enable ssh` | Start automatically at boot |
| `sudo systemctl disable ssh` | Do not start at boot |
| `systemctl list-units --type=service --state=running` | Running services |
| `systemctl --failed` | Failed services |

---

## 9. Scheduling with cron

`crontab -e` edits your scheduled jobs. Each line has five time fields and a command:

```
┌ minute (0-59)
│ ┌ hour (0-23)
│ │ ┌ day of month (1-31)
│ │ │ ┌ month (1-12)
│ │ │ │ ┌ day of week (0-7, 0 and 7 are Sunday)
* * * * *  command
```

| Entry | Meaning |
| ----- | ------- |
| `0 2 * * * /home/kali/backup.sh` | Every day at 02:00 |
| `*/15 * * * * /home/kali/check.sh` | Every 15 minutes |
| `0 9 * * 1 /home/kali/report.sh` | Every Monday at 09:00 |

`crontab -l` lists your jobs and `crontab -r` removes them all.

---

## Practice

1. Start `sleep 300 &`, find its PID with `pgrep`, then stop it with `kill`.
2. Start `sleep 300`, suspend it with `Ctrl+Z`, resume it in the background with `bg` and list it with `jobs`.
3. Show the five processes using the most memory.
4. Find the ten largest files under your home directory.
5. Check disk usage and available memory in one line.
6. Check whether the `ssh` service is running and whether it starts at boot.
7. Write a cron line that runs a script every day at 02:00.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| `kill -9` as the first step | No cleanup, possible data loss | Try plain `kill` first |
| `pkill -9 python3` | Kills every Python process, including other people's work | Use the PID, or match more precisely with `pkill -f name` |
| Reading `free` instead of `available` | Looks like memory is almost gone | Read the `available` column |
| `kill 1` or killing `systemd` | Can crash the system | Never signal PID 1 |
| Cron job with relative paths | Fails because cron has a minimal environment | Use full paths in cron entries |

---

[Previous: File Viewing](../03-file-viewing/README.md) · [Back to index](../../README.md) · [Next: Users & Permissions](../05-users-and-permissions/README.md)
