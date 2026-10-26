# Module 6: System Logs

**Objective:** Find and read system logs for troubleshooting and security checks, using both log files in `/var/log` and the systemd journal.

| Section | Topic |
| ------- | ----- |
| [1](#1-where-logs-live) | Where logs live |
| [2](#2-common-log-files) | Common log files |
| [3](#3-reading-log-files) | Reading and searching log files |
| [4](#4-login-history) | Login history: `last` `lastb` `lastlog` |
| [5](#5-the-systemd-journal) | `journalctl` and `dmesg` |
| [6](#6-package-logs) | Package logs |
| [7](#7-log-rotation) | `logrotate` |
| [8](#practice) | Practice, common mistakes |

---

## 1. Where logs live

Logs record what the kernel, services and users do. They are used for:

- **Troubleshooting:** failed services, hardware and driver errors
- **Security monitoring:** failed logins, `sudo` use, suspicious activity
- **Auditing and forensics:** reconstructing what happened and when

There are two sources on a modern Debian-based system:

| Source | How to read it |
| ------ | -------------- |
| Text files in `/var/log` (written by `rsyslog` and by programs) | `cat`, `less`, `tail`, `grep` |
| The systemd journal (binary, collected by `systemd-journald`) | `journalctl` |

> **Note:** Recent Debian and Kali releases do not install `rsyslog` by default, so `/var/log/auth.log` and `/var/log/syslog` may not exist. Either read the journal with `journalctl`, or install the file-based logs with `sudo apt install rsyslog`.

---

## 2. Common log files

| File | Contents | Example use |
| ---- | -------- | ----------- |
| `/var/log/auth.log` | Logins, `sudo` and SSH authentication | Failed login attempts |
| `/var/log/syslog` (or `/var/log/messages`) | General system messages | Service problems |
| `/var/log/kern.log` | Kernel messages | USB and driver errors |
| `/var/log/dmesg` | Kernel ring buffer from boot | Boot-time hardware detection |
| `/var/log/boot.log` | Boot messages from services | Startup problems |
| `/var/log/wtmp` | Successful logins (binary) | Read with `last` |
| `/var/log/btmp` | Failed logins (binary) | Read with `lastb` |
| `/var/log/lastlog` | Last login per user (binary) | Read with `lastlog` |
| `/var/log/faillog` | Failed login counts (binary) | Read with `faillog` |
| `/var/log/dpkg.log` | Package installs and removals | Software changes |
| `/var/log/apt/history.log` | APT command history | What was installed and when |

Most of these need `sudo` to read.

---

## 3. Reading log files

| Command | What it does | Example |
| ------- | ------------ | ------- |
| `sudo tail -f <file>` | Follow a log in real time | `sudo tail -f /var/log/auth.log` |
| `sudo tail -n 50 <file>` | Last 50 lines | `sudo tail -n 50 /var/log/syslog` |
| `less <file>` | Scroll through a large log | `sudo less /var/log/syslog` |
| `grep <pattern> <file>` | Search a log | `sudo grep -i "usb" /var/log/kern.log` |
| `zcat <file>.gz` | Read a rotated, compressed log | `zcat /var/log/syslog.2.gz` |
| `zgrep <pattern> <file>.gz` | Search a compressed log | `zgrep "error" /var/log/syslog.2.gz` |

```bash
sudo tail -f /var/log/auth.log
```
```
Failed password for root from 192.168.1.10 port 22 ssh2
```

This line means someone tried to log in as `root` over SSH from `192.168.1.10` with a wrong password.

**Failed logins per source IP, most frequent first**

```bash
sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

---

## 4. Login history

| Command | What it shows |
| ------- | ------------- |
| `last` | Successful logins and reboots (from `wtmp`) |
| `sudo lastb` | Failed login attempts (from `btmp`) |
| `lastlog` | Last login time of every user |
| `faillog` | Failed login counts per user |
| `who`, `w` | Who is logged in now |

---

## 5. The systemd journal

| Command | What it does |
| ------- | ------------ |
| `journalctl` | All journal entries, oldest first |
| `journalctl -e` | Jump to the end |
| `journalctl -f` | Follow new entries (like `tail -f`) |
| `journalctl -b` | Entries since the current boot |
| `journalctl -b -1` | Entries from the previous boot |
| `journalctl -u ssh` | Entries of one service |
| `journalctl -p err` | Priority `err` and worse |
| `journalctl -k` | Kernel messages only |
| `journalctl --since "1 hour ago"` | Time filter |
| `journalctl --since "2026-01-01" --until "2026-01-02"` | Date range |
| `journalctl --disk-usage` | Space used by the journal |

Priority levels from most to least severe: `emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`.

`dmesg` prints the kernel ring buffer:

| Command | What it does |
| ------- | ------------ |
| `sudo dmesg \| less` | Scroll through kernel messages |
| `sudo dmesg -T` | Readable timestamps |
| `sudo dmesg --level=err,warn` | Errors and warnings only |

**Failed SSH logins from the journal**

```bash
sudo journalctl -u ssh | grep "Failed password"
```

**Write your own entry**

```bash
logger "Backup finished"
journalctl -n 1
```

---

## 6. Package logs

| File | What it shows |
| ---- | ------------- |
| `/var/log/apt/history.log` | Each `apt` transaction with the command line |
| `/var/log/dpkg.log` | Every package state change |

```bash
grep " install " /var/log/dpkg.log | tail
```

---

## 7. Log rotation

Logs grow without limit, so `logrotate` rotates, compresses and deletes old ones.

| Path | Purpose |
| ---- | ------- |
| `/etc/logrotate.conf` | Global settings |
| `/etc/logrotate.d/` | One file per service |

A sample rule:

```
/var/log/myapp/*.log {
    weekly
    rotate 4
    compress
    missingok
    notifempty
}
```

This rotates the logs weekly, keeps four old copies, compresses them, and does not complain if the file is missing or empty.

Test a configuration without changing anything: `sudo logrotate -d /etc/logrotate.conf`.

The journal has its own size limit in `/etc/systemd/journald.conf`. Trim it manually with `sudo journalctl --vacuum-time=7d`.

---

## Practice

1. Check whether `/var/log/auth.log` exists on your system. If not, read authentication events with `journalctl`.
2. Show the last 20 kernel messages with timestamps.
3. List every successful login with `last` and every failed one with `sudo lastb`.
4. Show only errors since the current boot.
5. Generate a failed `sudo` attempt (wrong password), then find it in the logs.
6. Write a custom entry with `logger` and find it again.
7. Show what you installed most recently with `apt` using `/var/log/apt/history.log`.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| Looking for `auth.log` on a new Kali | `No such file or directory` | Use `journalctl` or install `rsyslog` |
| Using `cat` on a large or live log | Floods the screen | Use `less`, `tail` or `tail -f` |
| Reading `wtmp` or `btmp` with `cat` | Garbled binary output | Use `last` and `lastb` |
| Forgetting `sudo` | `Permission denied` | Most logs are root-readable only |
| Deleting logs to free space | Loses evidence | Rotate or vacuum instead |

---

[Previous: Users & Permissions](../05-users-and-permissions/README.md) · [Back to index](../../README.md) · [Next: APT Package Management](../07-apt-package-management/README.md)
