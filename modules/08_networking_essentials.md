# Module 8: Networking

**Objective:** Check connectivity, read network configuration, resolve names, inspect ports and connections, transfer files and troubleshoot problems from the command line.

| Section | Topic |
| ------- | ----- |
| [1](#1-testing-connectivity) | `ping` |
| [2](#2-network-configuration) | `ip` `hostname` `nmcli` |
| [3](#3-dns) | `nslookup` `dig` `host` |
| [4](#4-tracing-the-path) | `traceroute` `mtr` |
| [5](#5-ports-and-connections) | `ss` `netstat` `lsof` |
| [6](#6-web-requests-and-downloads) | `curl` `wget` |
| [7](#7-remote-access-and-file-transfer) | `ssh` `scp` `sftp` `rsync` |
| [8](#8-scanning-and-capturing) | `nc` `nmap` `tcpdump` |
| [9](#9-firewall-basics) | `ufw` |
| [10](#10-key-concepts) | Concepts and port numbers |
| [11](#11-troubleshooting-workflow) | Troubleshooting |
| [12](#12-windows-equivalents) | Windows equivalents |
| [13](#practice) | Practice, common mistakes |

Outputs shown here are examples. Your addresses and times will differ.

---

## 1. Testing connectivity

`ping` sends ICMP echo requests and reports whether the host answers and how long it takes.

| Command | What it does |
| ------- | ------------ |
| `ping 8.8.8.8` | Ping an IP address (runs until `Ctrl+C`) |
| `ping -c 4 google.com` | Send 4 packets |
| `ping -i 2 google.com` | Wait 2 seconds between packets |
| `ping -s 1000 google.com` | Set the payload size |
| `ping -c 4 127.0.0.1` | Test your own network stack |

```bash
ping -c 4 8.8.8.8
```
```
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 9.8/10.4/11.2/0.5 ms
```

A reply proves the host is reachable. `time` is the latency and packet loss shows reliability. Some hosts block ICMP, so no reply does not always mean the host is down.

---

## 2. Network configuration

| Command | What it does |
| ------- | ------------ |
| `ip addr` | IP addresses of all interfaces |
| `ip -br addr` | Short, one line per interface |
| `ip link` | Interface state and MAC address |
| `ip route` | Routing table and default gateway |
| `ip neigh` | ARP table: IP to MAC mappings |
| `hostname` | Computer name |
| `hostname -I` | All IP addresses of this host |
| `nmcli device status` | Interface status in NetworkManager |
| `sudo ip link set eth0 up` | Bring an interface up |
| `sudo ip addr add 192.168.0.50/24 dev eth0` | Add an address (lost at reboot) |

`ifconfig` still works if the `net-tools` package is installed, but it is deprecated. Use `ip`.

```bash
ip -br addr
```
```
lo      UNKNOWN  127.0.0.1/8
eth0    UP       192.168.0.101/24
```

| Information | Example | Meaning |
| ----------- | ------- | ------- |
| IP address | `192.168.0.101/24` | Address and prefix length |
| Subnet mask | `255.255.255.0` | Same as `/24` |
| Default gateway | `192.168.0.1` | Router to other networks (`ip route`) |
| MAC address | `08:00:27:76:d7:69` | Hardware address of the card |
| Broadcast | `192.168.0.255` | Address that reaches every host in the subnet |
| MTU | `1500` | Largest packet the interface sends |

Interface flags in `ip link`: `UP` interface enabled, `LOWER_UP` cable or Wi-Fi connected, `NO-CARRIER` no connection, `BROADCAST` and `MULTICAST` supported.

Interface names: `lo` is the loopback (127.0.0.1, talks to itself), `eth0` or `enp0s3` is wired Ethernet, `wlan0` or `wlp2s0` is Wi-Fi.

---

## 3. DNS

DNS translates names such as `google.com` into IP addresses.

| Command | What it does |
| ------- | ------------ |
| `nslookup google.com` | Look up a name |
| `nslookup 8.8.8.8` | Reverse lookup |
| `host google.com` | Short lookup |
| `dig google.com` | Detailed lookup |
| `dig +short google.com` | Only the answer |
| `dig google.com MX` | Mail servers |
| `dig google.com AAAA` | IPv6 address |
| `dig google.com TXT` | Text records (SPF, verification) |
| `dig @8.8.8.8 google.com` | Ask a specific DNS server |
| `dig +trace google.com` | Follow resolution from the root servers |

| Record | Purpose | Example answer |
| ------ | ------- | -------------- |
| `A` | IPv4 address | `142.250.4.139` |
| `AAAA` | IPv6 address | `2001:db8::1` (documentation example) |
| `MX` | Mail server | `10 smtp.example.com.` |
| `CNAME` | Alias to another name | `www.example.com → example.com` |
| `TXT` | Free text | `v=spf1 ...` |
| `NS` | Name servers of the zone | `ns1.example.com.` |

In `dig` output look at the **ANSWER SECTION** (the result), the **TTL** (how long it may be cached), the **Query time** and the **SERVER** that answered.

Local name files:

| File | Purpose |
| ---- | ------- |
| `/etc/hosts` | Static name-to-IP entries, checked first |
| `/etc/resolv.conf` | DNS servers in use (often managed by systemd-resolved or NetworkManager, so manual edits may be overwritten) |

---

## 4. Tracing the path

| Command | What it does |
| ------- | ------------ |
| `traceroute google.com` | Show the routers between you and the destination |
| `traceroute -n google.com` | Do not resolve names, faster |
| `traceroute -m 10 google.com` | Maximum 10 hops |
| `traceroute -q 1 google.com` | One probe per hop instead of three |
| `mtr google.com` | Live traceroute with loss statistics per hop |

```bash
traceroute -n google.com
```
```
traceroute to google.com (142.250.4.139), 30 hops max, 60 byte packets
 1  192.168.0.1  1.2 ms  1.1 ms  1.3 ms
 2  10.50.0.1    5.2 ms  5.0 ms  5.3 ms
 3  ...
```

Each line is a **hop** (a router). The three times are three probes. A sudden jump in latency at one hop points at a bottleneck. `* * *` means that hop did not answer, which is common and not always a problem.

---

## 5. Ports and connections

`ss` is the modern replacement for `netstat`.

| Command | What it does |
| ------- | ------------ |
| `sudo ss -tulnp` | Listening TCP and UDP ports with the owning process |
| `ss -tn` | Established TCP connections, numeric |
| `ss -s` | Summary statistics |
| `sudo netstat -tulnp` | Same as `ss -tulnp` (needs `net-tools`) |
| `sudo lsof -i :22` | What is using port 22 |
| `sudo lsof -i -P -n` | All network connections |

| Flag | Meaning |
| ---- | ------- |
| `-t` / `-u` | TCP / UDP |
| `-l` | Listening sockets only |
| `-n` | Numeric (no name lookups) |
| `-p` | Show the process |

```bash
sudo ss -tulnp
```
```
Netid State  Recv-Q Send-Q Local Address:Port  Peer Address:Port Process
tcp   LISTEN 0      128    0.0.0.0:22          0.0.0.0:*         users:(("sshd",pid=1234,fd=3))
tcp   LISTEN 0      80     127.0.0.1:3306      0.0.0.0:*         users:(("mysqld",pid=5678,fd=21))
udp   UNCONN 0      0      0.0.0.0:68          0.0.0.0:*         users:(("dhclient",pid=1111,fd=6))
```

- `0.0.0.0:22` means SSH listens on every interface.
- `127.0.0.1:3306` means MySQL is reachable only from this machine.
- `UNCONN` is the normal state of a UDP socket waiting for packets.

| Feature | TCP | UDP |
| ------- | --- | --- |
| Connection | Handshake first | None |
| Delivery | Guaranteed and ordered | Best effort |
| Typical use | Web, SSH, email, file transfer | DNS, DHCP, streaming, games |

---

## 6. Web requests and downloads

### curl

| Command | What it does |
| ------- | ------------ |
| `curl https://example.com` | Print the response body |
| `curl -I https://example.com` | Headers and status code only |
| `curl -v https://example.com` | Verbose, shows the whole exchange |
| `curl -s -o /dev/null -w "%{http_code}\n" https://example.com` | Print only the status code |
| `curl -L <url>` | Follow redirects |
| `curl -o file.zip <url>` | Save to a file |
| `curl -X POST -d "a=1" <url>` | Send a POST request |
| `curl -u user:pass <url>` | Basic authentication |

Common status codes: `200` OK, `301` and `302` redirects, `403` forbidden, `404` not found, `500` server error.

### wget

| Command | What it does |
| ------- | ------------ |
| `wget <url>` | Download a file |
| `wget -c <url>` | Resume an interrupted download |
| `wget -O name.txt <url>` | Save under another name |

Use `curl` to inspect and test servers and APIs. Use `wget` mainly to download files.

---

## 7. Remote access and file transfer

| Command | What it does |
| ------- | ------------ |
| `ssh user@192.168.0.50` | Log in to a remote machine |
| `ssh -p 2222 user@host` | Use another port |
| `ssh-keygen -t ed25519` | Create a key pair |
| `ssh-copy-id user@host` | Install your public key on the server |
| `scp file.pdf user@host:/home/user/` | Copy a file to a remote host |
| `scp user@host:/home/user/data.txt .` | Copy a file from a remote host |
| `scp -r folder user@host:/tmp/` | Copy a directory |
| `sftp user@host` | Interactive file transfer session |
| `rsync -avz folder/ user@host:/backup/` | Efficient sync, only copies changes |

`scp` is considered a legacy protocol by the OpenSSH project. `sftp` and `rsync` over SSH are the recommended alternatives, but `scp` is still widely used. All of these are encrypted, unlike plain FTP.

Private SSH keys must be private: `chmod 600 ~/.ssh/id_ed25519`.

---

## 8. Scanning and capturing

> Scan and capture only on systems and networks that you own or have written permission to test.

| Command | What it does |
| ------- | ------------ |
| `nc -zv host 22` | Check if a TCP port is open |
| `nc -lvnp 4444` | Listen on a port (for testing connections) |
| `nmap 192.168.0.1` | Scan the common ports of a host |
| `nmap -sn 192.168.0.0/24` | Find live hosts on a subnet |
| `nmap -sV host` | Detect service versions |
| `sudo tcpdump -i eth0 -c 10` | Capture 10 packets |
| `sudo tcpdump -i eth0 port 53` | Capture DNS traffic only |

---

## 9. Firewall basics

`ufw` (Uncomplicated Firewall) is a simple front end for the Linux firewall. Install it with `sudo apt install ufw` if needed.

| Command | What it does |
| ------- | ------------ |
| `sudo ufw status verbose` | Show rules and state |
| `sudo ufw allow 22/tcp` | Allow SSH |
| `sudo ufw deny 23` | Block Telnet |
| `sudo ufw enable` | Turn the firewall on |
| `sudo ufw disable` | Turn it off |

Allow SSH **before** enabling the firewall on a remote machine, or you will lock yourself out.

---

## 10. Key concepts

**Private address ranges**

| Range | Class |
| ----- | ----- |
| `10.0.0.0/8` | Large private networks |
| `172.16.0.0/12` | Medium private networks |
| `192.168.0.0/16` | Home and small office |

**NAT.** A router replaces the private source address of outgoing packets with its public address, and keeps a table so replies are sent back to the right internal host. Servers on the internet see only your public address.

**IPv4 vs IPv6**

| | IPv4 | IPv6 |
| - | ---- | ---- |
| Format | `192.168.0.101` | `2001:db8::1` |
| Size | 32 bits | 128 bits |
| Note | Public addresses are exhausted | Addresses starting with `fe80::` are link-local, valid only on the local network |

**Well-known ports**

| Port | Service | Port | Service |
| ---- | ------- | ---- | ------- |
| 20/21 | FTP | 80 | HTTP |
| 22 | SSH | 110 | POP3 |
| 23 | Telnet | 143 | IMAP |
| 25 | SMTP | 443 | HTTPS |
| 53 | DNS | 3306 | MySQL |
| 67/68 | DHCP | 3389 | RDP |

---

## 11. Troubleshooting workflow

Test from the inside out, one layer at a time.

| Step | Command | If it fails |
| ---- | ------- | ----------- |
| 1. Is the interface up with an IP? | `ip -br addr` | Check the cable or Wi-Fi, restart NetworkManager |
| 2. Is there a default gateway? | `ip route` | Check DHCP or set the gateway |
| 3. Can you reach the router? | `ping -c 4 192.168.0.1` | Local network problem |
| 4. Can you reach the internet by IP? | `ping -c 4 8.8.8.8` | Router or ISP problem |
| 5. Does DNS work? | `ping -c 4 google.com` or `dig google.com` | If step 4 worked, this is a DNS problem |
| 6. Is the service reachable? | `curl -I https://site.com` | Firewall, server down or wrong port |
| 7. Where does it slow down? | `traceroute google.com` or `mtr google.com` | Look for the hop where latency jumps |

Useful fixes:

```bash
sudo systemctl restart NetworkManager
cat /etc/resolv.conf
sudo resolvectl status
```

---

## 12. Windows equivalents

| Task | Linux | Windows |
| ---- | ----- | ------- |
| IP configuration | `ip addr` | `ipconfig /all` |
| Routing table | `ip route` | `route print` |
| Trace route | `traceroute` | `tracert` |
| Trace with loss statistics | `mtr` | `pathping` |
| ARP table | `ip neigh` or `arp -a` | `arp -a` |
| Open ports | `ss -tulnp` | `netstat -ano` |
| DNS lookup | `dig`, `nslookup` | `nslookup` |
| MAC address | `ip link` | `getmac /v` |
| System information | `uname -a`, `hostnamectl` | `systeminfo` |

---

## Practice

1. Show your IP address, subnet prefix and default gateway.
2. Ping the gateway, then `8.8.8.8`, then `google.com`. Explain what each result tells you.
3. Find the IPv4 address and the mail servers of a domain with `dig`.
4. Trace the route to a website and identify your router in hop 1.
5. List the listening ports on your machine and name the process behind each one.
6. Check the HTTP status code of a website with `curl`.
7. Start a simple listener with `nc -lvnp 4444` and connect to it from a second terminal with `nc 127.0.0.1 4444`.
8. Copy a file to another machine or VM with `scp`.

## Common mistakes

| Mistake | What happens | Fix |
| ------- | ------------ | --- |
| Treating a failed `ping` as proof the host is down | The host may block ICMP | Confirm with `curl` or `nc -zv host port` |
| Editing `/etc/resolv.conf` by hand | Reset after a reboot or network change | Change DNS in NetworkManager or systemd-resolved |
| Using `ifconfig` or `netstat` on a fresh install | `command not found` | Use `ip` and `ss`, or install `net-tools` |
| Enabling `ufw` over SSH without allowing SSH | You lose access | `sudo ufw allow 22/tcp` first |
| Scanning networks you do not own | Illegal and detectable | Scan only with permission |

---

[Previous: APT Package Management](../07-apt-package-management/README.md) · [Back to index](../../README.md)
