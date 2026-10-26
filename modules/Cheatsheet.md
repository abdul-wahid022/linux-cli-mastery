# Linux CLI Cheat Sheet

One-page reference for the commands covered in the [modules](./README.md#modules).

## Files and directories ([Module 1](./modules/01-files-and-directories/README.md))

| Task | Command |
| ---- | ------- |
| Current directory | `pwd` |
| List with details, hidden files, readable sizes | `ls -lah` |
| Change directory / back / home | `cd dir` · `cd -` · `cd ~` |
| Create directories (nested) | `mkdir -p a/b/c` |
| Create an empty file | `touch file` |
| Copy file / directory | `cp a b` · `cp -r dir/ dest/` |
| Move or rename | `mv old new` |
| Delete file / empty dir / directory tree | `rm f` · `rmdir d` · `rm -r d` |
| Find by name | `find . -name "*.txt"` |
| Directory tree | `tree -L 2` |
| Symbolic link | `ln -s target link` |
| Help | `man cmd` · `cmd --help` |

## Compression and archives ([Module 2](./modules/02-compression-and-archiving/README.md))

| Task | Command |
| ---- | ------- |
| Create `.tar.gz` | `tar -czvf a.tar.gz dir/` |
| Extract `.tar.gz` | `tar -xzvf a.tar.gz` |
| Extract elsewhere | `tar -xzvf a.tar.gz -C /tmp/out` |
| List an archive | `tar -tvf a.tar` |
| Compress, keep original | `gzip -k file` |
| Zip / unzip | `zip -r a.zip dir/` · `unzip a.zip` |
| Checksum | `sha256sum file` |

## Viewing and text tools ([Module 3](./modules/03-file-viewing/README.md))

| Task | Command |
| ---- | ------- |
| Whole file / paged | `cat f` · `less f` |
| First / last lines | `head -n 20 f` · `tail -n 20 f` |
| Follow a log | `tail -f log` |
| Search (case-insensitive, recursive, line numbers) | `grep -irn "text" dir/` |
| Lines that do not match | `grep -v "text" f` |
| Count lines / words | `wc -l f` · `wc -w f` |
| Compare files | `diff -u a b` |
| Sort and count duplicates | `sort f \| uniq -c \| sort -nr` |
| Redirect output / append | `cmd > f` · `cmd >> f` |
| Discard errors | `cmd 2>/dev/null` |

## System and processes ([Module 4](./modules/04-system-and-processes/README.md))

| Task | Command |
| ---- | ------- |
| All processes | `ps aux` |
| Live view | `top` · `htop` |
| PID by name | `pgrep name` |
| Stop / force stop | `kill PID` · `kill -9 PID` |
| Stop by name | `pkill name` |
| Run in background / list jobs | `cmd &` · `jobs` |
| Suspend, resume | `Ctrl+Z` · `bg %1` · `fg %1` |
| Uptime and load | `uptime` |
| Disk space / folder size | `df -h` · `du -sh dir/` |
| Memory | `free -h` |
| Service status / restart / enable at boot | `systemctl status s` · `sudo systemctl restart s` · `sudo systemctl enable s` |
| Edit scheduled jobs | `crontab -e` |

## Users and permissions ([Module 5](./modules/05-users-and-permissions/README.md))

| Task | Command |
| ---- | ------- |
| Permissions of files | `ls -l` |
| Make executable | `chmod +x script.sh` |
| Numeric mode | `chmod 644 file` · `chmod 755 file` · `chmod 600 file` |
| Change owner and group | `sudo chown user:group file` |
| Current user and groups | `whoami` · `id` |
| Create user / set password | `sudo adduser name` · `sudo passwd name` |
| Add user to a group | `sudo usermod -aG group name` |
| Run as root | `sudo cmd` |
| Default permissions | `umask` |

Permission values: `r = 4`, `w = 2`, `x = 1`.

## Logs ([Module 6](./modules/06-system-logs/README.md))

| Task | Command |
| ---- | ------- |
| Follow auth log | `sudo tail -f /var/log/auth.log` |
| Journal for a service | `journalctl -u ssh` |
| Errors since boot | `journalctl -b -p err` |
| Follow the journal | `journalctl -f` |
| Kernel messages | `sudo dmesg -T` |
| Logins / failed logins | `last` · `sudo lastb` |
| Read rotated log | `zcat file.gz` |

## APT ([Module 7](./modules/07-apt-package-management/README.md))

| Task | Command |
| ---- | ------- |
| Refresh lists | `sudo apt update` |
| Upgrade | `sudo apt upgrade` · `sudo apt full-upgrade` |
| Install / remove / purge | `sudo apt install p` · `sudo apt remove p` · `sudo apt purge p` |
| Search / info | `apt search w` · `apt show p` |
| Remove unused / clear cache | `sudo apt autoremove` · `sudo apt clean` |
| Files of a package / owner of a file | `dpkg -L p` · `dpkg -S /path` |
| Fix broken installs | `sudo apt --fix-broken install` |

## Networking ([Module 8](./modules/08-networking/README.md))

| Task | Command |
| ---- | ------- |
| IP addresses | `ip -br addr` |
| Routes / gateway | `ip route` |
| Connectivity | `ping -c 4 8.8.8.8` |
| DNS lookup | `dig +short google.com` |
| Trace route | `traceroute -n host` · `mtr host` |
| Listening ports | `sudo ss -tulnp` |
| Check a port | `nc -zv host 22` |
| HTTP status / headers | `curl -I https://site` |
| Download | `wget -c url` |
| Remote login | `ssh user@host` |
| Copy files securely | `scp f user@host:/path/` · `rsync -avz dir/ user@host:/dest/` |

## Shell shortcuts

| Key | Action |
| --- | ------ |
| `Tab` | Complete names |
| `Ctrl+C` | Cancel the running command |
| `Ctrl+D` | End input / log out |
| `Ctrl+L` | Clear the screen |
| `Ctrl+R` | Search history |
| `Ctrl+A` / `Ctrl+E` | Start / end of line |
| `!!` | Repeat the previous command |
