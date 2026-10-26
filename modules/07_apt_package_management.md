# Module 7: APT Package Management

**Objective:** Install, update, remove and troubleshoot software on Debian-based systems (Kali, Debian, Ubuntu, Linux Mint).

| Section | Topic |
| ------- | ----- |
| [1](#1-what-apt-does) | What APT does |
| [2](#2-everyday-commands) | Everyday commands |
| [3](#3-searching-and-inspecting) | Searching and inspecting |
| [4](#4-cleaning-up) | Cleaning up |
| [5](#5-flags) | Flags |
| [6](#6-working-with-dpkg) | `dpkg` |
| [7](#7-holding-packages-and-repositories) | Holding packages and repositories |
| [8](#8-troubleshooting) | Troubleshooting |
| [9](#practice) | Practice, common mistakes |

This module applies to Debian-based systems only. Fedora and Red Hat use `dnf`, Arch uses `pacman`.

---

## 1. What APT does

APT (Advanced Package Tool) downloads packages from repositories, resolves their dependencies, installs them and keeps them updated. Commands that change the system need `sudo`. Read-only commands such as `apt search` and `apt show` do not.

| Tool | Role |
| ---- | ---- |
| `apt` | Friendly command for interactive use (progress bar, colours) |
| `apt-get`, `apt-cache` | Older commands with stable output, preferred in scripts |
| `dpkg` | Low-level tool that installs individual `.deb` files, without resolving dependencies |

---

## 2. Everyday commands

| Command | What it does |
| ------- | ------------ |
| `sudo apt update` | Refresh the package lists from the repositories. Installs nothing. |
| `sudo apt upgrade` | Upgrade installed packages. Never removes packages. |
| `sudo apt full-upgrade` | Upgrade and allow removing or installing packages when needed |
| `sudo apt install <pkg>` | Install a package and its dependencies |
| `sudo apt reinstall <pkg>` | Reinstall a package |
| `sudo apt remove <pkg>` | Remove a package, keep its configuration files |
| `sudo apt purge <pkg>` | Remove a package and its configuration files |
| `sudo apt autoremove` | Remove dependencies that nothing needs any more |

Always run `update` before `upgrade`. On rolling releases such as Kali, `full-upgrade` is the usual choice.

```bash
sudo apt update
```
```
Hit:1 http://http.kali.org/kali kali-rolling InRelease
Reading package lists... Done
```

```bash
sudo apt install nmap
```
```
The following NEW packages will be installed:
  nmap
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
```

---

## 3. Searching and inspecting

| Command | What it does |
| ------- | ------------ |
| `apt search <word>` | Search names and descriptions |
| `apt show <pkg>` | Version, size, dependencies, description |
| `apt policy <pkg>` | Installed and available versions |
| `apt list --installed` | All installed packages |
| `apt list --upgradable` | Packages with updates waiting |
| `apt-cache depends <pkg>` | What a package depends on |
| `apt-cache rdepends <pkg>` | What depends on a package |
| `apt download <pkg>` | Download the `.deb` without installing |

```bash
apt list --installed | grep -i nmap
```

---

## 4. Cleaning up

| Command | What it does |
| ------- | ------------ |
| `sudo apt autoremove` | Remove unused dependencies |
| `sudo apt clean` | Delete all downloaded `.deb` files from `/var/cache/apt/archives/` |
| `sudo apt autoclean` | Delete only cached files that can no longer be downloaded |

---

## 5. Flags

| Flag | Meaning | Example |
| ---- | ------- | ------- |
| `-y` | Answer "yes" to prompts | `sudo apt install nmap -y` |
| `-s` | Simulate, change nothing | `sudo apt upgrade -s` |
| `-f` | Fix broken dependencies | `sudo apt install -f` |
| `--no-install-recommends` | Install only required dependencies | `sudo apt install <pkg> --no-install-recommends` |

Use `-y` only when you already know what will be installed or removed. Run with `-s` first if unsure.

---

## 6. Working with dpkg

| Command | What it does |
| ------- | ------------ |
| `sudo dpkg -i package.deb` | Install a local `.deb` file |
| `sudo apt install ./package.deb` | Install a local `.deb` **and** resolve dependencies (preferred) |
| `dpkg -l` | List installed packages |
| `dpkg -l \| grep <name>` | Check if a package is installed |
| `dpkg -L <pkg>` | List the files a package installed |
| `dpkg -S /usr/bin/nmap` | Which package owns this file |
| `sudo dpkg -r <pkg>` | Remove a package |
| `sudo dpkg --configure -a` | Finish interrupted installations |

---

## 7. Holding packages and repositories

| Command | What it does |
| ------- | ------------ |
| `sudo apt-mark hold <pkg>` | Stop a package from being upgraded |
| `sudo apt-mark unhold <pkg>` | Allow upgrades again |
| `apt-mark showhold` | List held packages |

Repositories are listed in:

| Path | Purpose |
| ---- | ------- |
| `/etc/apt/sources.list` | Main repository list |
| `/etc/apt/sources.list.d/` | Extra repositories, one file each |

Edit them with `sudo apt edit-sources` and run `sudo apt update` afterwards. Third-party repositories need their signing key. Current guidance is to store keys in `/etc/apt/keyrings/` and reference them with `signed-by=` in the source entry. The old `apt-key` command is deprecated.

`add-apt-repository ppa:<name>` is for Ubuntu PPAs. It is not available by default on Kali and PPAs are not meant for Kali.

---

## 8. Troubleshooting

### "Could not get lock /var/lib/dpkg/lock-frontend"

Another package process is running. Wait for it, or check:

```bash
ps aux | grep -i -E "apt|dpkg"
sudo lsof /var/lib/dpkg/lock-frontend
```

Remove lock files only if no APT or dpkg process is running, then repair:

```bash
sudo rm /var/lib/dpkg/lock-frontend /var/lib/dpkg/lock
sudo dpkg --configure -a
sudo apt update
```

### "You have held broken packages"

```bash
sudo apt --fix-broken install
sudo dpkg --configure -a
sudo apt update
```

### "Hash Sum mismatch"

```bash
sudo rm -rf /var/lib/apt/lists/*
sudo apt clean
sudo apt update
```

### "Failed to fetch" or "Unable to locate package"

- Check the internet connection and DNS.
- Run `sudo apt update` and try again.
- Check `/etc/apt/sources.list` for typos or an outdated mirror.

---

## Practice

1. Update the package lists, then list the packages that can be upgraded.
2. Simulate an upgrade with `-s` and read the summary.
3. Install `tree`, run it, then show which files the package installed with `dpkg -L tree`.
4. Find which package provides `/usr/bin/tree` with `dpkg -S`.
5. Remove `tree` and check what is left behind. Then purge it.
6. Hold a package, confirm it with `apt-mark showhold`, then unhold it.
7. Free disk space with `autoremove` and `clean`.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| `apt upgrade` without `apt update` | Works from stale lists | Run `update` first |
| Using `-y` blindly | May remove packages without you noticing | Read the summary, or simulate with `-s` |
| `dpkg -i` with missing dependencies | Package is left half-installed | `sudo apt install -f`, or install with `apt install ./file.deb` |
| Deleting lock files while apt runs | Corrupts the package database | Wait for the other process first |
| Adding Ubuntu PPAs to Kali | Breaks dependencies | Use Kali repositories or install from source |
| `purge` on a package with data you need | Configuration is deleted | Back up first, or use `remove` |

---

[Previous: System Logs](../06-system-logs/README.md) · [Back to index](../../README.md) · [Next: Networking](../08-networking/README.md)
