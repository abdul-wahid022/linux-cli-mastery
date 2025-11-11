# Module 8: Networking Essentials

Networking commands are fundamental tools for system administrators, network engineers, and cybersecurity professionals. These commands help you diagnose network issues, test connectivity, analyze traffic, and manage network configurations. Mastering these essential networking commands is crucial for maintaining reliable and secure network infrastructure.

### Table of Contents

1.  [Introduction to Networking Commands](#introduction-to-networking-commands)
2.  [Basic Connectivity Commands](#basic-connectivity-commands)
3.  [Network Configuration Commands](#network-configuration-commands)
4.  [DNS and Name Resolution](#dns-and-name-resolution)
5.  [Network Diagnostics and Troubleshooting](#network-diagnostics-and-troubleshooting)
6.  [Network Monitoring Commands](#network-monitoring-commands)
7.  [File Transfer Commands](#file-transfer-commands)
8.  [Advanced Networking Tools](#advanced-networking-tools)
9.  [Understanding Network Concepts](#understanding-network-concepts)
10. [Practical Examples](#practical-examples)
11. [Troubleshooting Common Issues](#troubleshooting-common-issues)
12. [Best Practices](#best-practices)
13. [Conclusion](#conclusion)

---

## Introduction to Networking Commands

Networking commands are command-line utilities that allow you to interact with and manage network interfaces, test connectivity, diagnose problems, and transfer data across networks. These tools are available on most Unix-like operating systems (Linux, macOS) and Windows (with some variations).

**Why Learn Networking Commands?**
*   **Troubleshooting:** Quickly identify and resolve network connectivity issues.
*   **Security:** Analyze network traffic and detect potential security threats.
*   **Performance:** Monitor network performance and optimize configurations.
*   **Administration:** Manage network interfaces, routing tables, and connections.

---

## Basic Connectivity Commands

These commands help you test basic network connectivity and verify if hosts are reachable.

### 1. ping - Test Network Connectivity

Tests connectivity to a host by sending ICMP echo requests.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `ping <host>` | Check if host is reachable | Host tak connectivity check karta hai | `ping 8.8.8.8` |
| `ping <domain>` | Check connectivity to domain | Domain tak connectivity test karta hai | `ping google.com` |
| `ping -c <count> <host>` | Limit number of ping packets | Specific number of packets bhejta hai | `ping -c 4 google.com` |
| `ping -i <interval> <host>` | Set interval between packets | Packets ke beech time set karta hai | `ping -i 2 google.com` |
| `ping -s <size> <host>` | Change packet size | Packet ka size change karta hai | `ping -s 1000 google.com` |

**What ping Does:**
*   Sends ICMP (Internet Control Message Protocol) echo requests to the target host.
*   Measures response time (latency) and displays packet loss statistics.
*   Confirms:
    1. The host is reachable
    2. Network latency (time lag)
    3. Packet loss percentage

**Practical Use Cases:**
*   **Check router connectivity:** `ping 192.168.0.1`
*   **Test internet connection:** `ping 8.8.8.8` (Google DNS)
*   **Diagnose DNS issues:** If `ping google.com` fails but `ping 8.8.8.8` works, it's a DNS problem

---

## Network Configuration Commands

These commands display and configure network interface settings.

### 1. ipconfig / ifconfig - Network Interface Configuration

**Windows:**

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `ipconfig` | Display basic network configuration | Basic network settings dikhata hai | `ipconfig` |
| `ipconfig /all` | Show detailed network information | Complete network details dikhata hai | `ipconfig /all` |

**Linux:**

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `ifconfig` | Display network interfaces (deprecated) | Network interfaces ka configuration dikhata hai | `ifconfig` |
| `ip addr` | Modern way to show IP addresses | Network interfaces aur IP addresses show karta hai | `ip addr` |
| `ip link` | Show network interfaces | Network interfaces ki list dikhata hai | `ip link` |

**Key Information Displayed:**
*   **IP Address:** Your device's network address (e.g., `192.168.0.101`)
*   **Subnet Mask:** Defines network range (e.g., `255.255.255.0`)
*   **Default Gateway:** Router IP that connects you to other networks (e.g., `192.168.0.1`)
*   **MAC Address:** Physical hardware address of network card
*   **DNS Servers:** Servers that translate domain names to IP addresses

### 2. hostname - Display Computer Name

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `hostname` | Show computer's network name | Computer ka naam dikhata hai | `hostname` |

### 3. getmac - Display MAC Address (Windows)

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `getmac /v` | Show MAC addresses with details | Network card ka physical address dikhata hai | `getmac /v` |

---

## DNS and Name Resolution

DNS (Domain Name System) translates human-readable domain names into IP addresses.

### Understanding DNS

**DNS = Internet's Phonebook**

*   When you type `google.com` in your browser:
    1. Your system asks DNS server: "What's the IP address of google.com?"
    2. DNS replies: `142.250.183.14`
    3. Your system connects to that IP address

### 1. nslookup - Query DNS Records

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `nslookup <domain>` | Lookup domain's IP address | Domain ka IP address pata karta hai | `nslookup google.com` |
| `nslookup <ip>` | Reverse lookup (IP to domain) | IP se domain name pata karta hai | `nslookup 8.8.8.8` |

**Example Output:**
```text
Server:  dns.google
Address: 8.8.8.8

Non-authoritative answer:
Name:    google.com
Addresses: 142.250.4.139
          142.250.4.102
          142.250.4.138
```

**What This Shows:**
*   **Server:** DNS server being used (8.8.8.8 = Google DNS)
*   **Addresses:** Multiple IP addresses for google.com (load balancing)

### 2. dig - Advanced DNS Lookup Tool

`dig` provides detailed DNS information with multiple sections.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `dig <domain>` | Detailed DNS query | Complete DNS information dikhata hai | `dig google.com` |
| `dig <domain> MX` | Query mail server records | Email server ka address pata karta hai | `dig google.com MX` |
| `dig <domain> AAAA` | Query IPv6 address | IPv6 address pata karta hai | `dig google.com AAAA` |
| `dig +short <domain>` | Show only IP address | Sirf IP address dikhata hai | `dig +short google.com` |
| `dig +trace <domain>` | Trace DNS resolution path | DNS path step-by-step dikhata hai | `dig +trace google.com` |

**DNS Record Types:**

| Record Type | Purpose | Example |
| :--- | :--- | :--- |
| **A** | IPv4 address | `google.com → 142.250.72.14` |
| **AAAA** | IPv6 address | `google.com → fe80::a00:27ff:fe76:d769` |
| **MX** | Mail server | Email ke liye server address |
| **TXT** | Extra info (SPF records) | Security aur verification info |

**dig Output Sections:**

1. **QUESTION SECTION:** What you asked DNS server
2. **ANSWER SECTION:** DNS server's reply (IP address)
3. **Query time:** How fast DNS responded
4. **TTL (Time To Live):** How long to cache this result (e.g., 299 seconds)
5. **SERVER:** Which DNS server answered your query

---

## Network Diagnostics and Troubleshooting

### 1. traceroute / tracert - Trace Network Path

Shows the route packets take from your computer to destination.

**Windows:**

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `tracert <host>` | Trace route to destination | Destination tak ka route dikhata hai | `tracert google.com` |

**Linux/macOS:**

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `traceroute <host>` | Trace route with details | Packets ka complete path dikhata hai | `traceroute google.com` |
| `traceroute -m <hops> <host>` | Limit maximum hops | Maximum hops limit karta hai | `traceroute -m 10 google.com` |
| `traceroute -q <queries> <host>` | Change packet size per hop | Har hop pe packets change karta hai | `traceroute -q 1 google.com` |

**Example Output:**
```text
Tracing route to google.com [142.250.4.139]
over a maximum of 30 hops:

1    1 ms    1 ms    1 ms    192.168.0.1
2    5 ms    5 ms    5 ms    10.50.0.1
3   20 ms   20 ms   20 ms    172.217.0.142
```

**Understanding the Output:**

| Hop | IP Address | Meaning |
| :--- | :--- | :--- |
| **1** | `192.168.0.1` | Your home router/gateway |
| **2** | `10.50.0.1` | ISP's internal router |
| **3** | `172.217.0.142` | Destination server |

*   **Three numbers (1 ms, 1 ms, 1 ms):** Response time for 3 test packets
*   **Hop:** Each router the packet passes through

**Real-World Analogy:**
Think of sending a parcel:
1. Your home → Local post office
2. Local post office → Regional office
3. Regional office → Destination city post office
4. Destination post office → Receiver's home

Each office = **hop**, delivery time = **latency**

**Traceroute vs Web Page Access:**

When you visit a website (e.g., google.com):
1. **DNS query:** Your system asks for google.com's IP
2. **Packet creation:** Your system creates a data packet with your local IP (e.g., `192.168.0.101`)
3. **Router forwarding:** Packet goes to your router
4. **NAT (Network Address Translation):** 
   - Your local IP is hidden
   - Router uses its public IP to send request
5. **Google server reply:** Responds to your router's public IP
6. **Router mapping:** Router checks its NAT table and forwards data to your PC (`192.168.0.101`)

**Security Note:** Destination servers only see your router's public IP, not your local private IP.

### 2. pathping - Enhanced Route Tracing (Windows)

Combines `ping` and `tracert` functionality.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `pathping <host>` | Trace route with packet loss stats | Route aur packet loss dono dikhata hai | `pathping google.com` |

**What It Does:**
*   Shows complete route path
*   Calculates packet loss for each hop over time
*   More comprehensive than `tracert`

### 3. route - Display/Manage Routing Table

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `route print` | Show routing table | System ka routing table dikhata hai | `route print` |
| `ip route` | Show routes (Linux) | Network routes dikhata hai | `ip route` |

**What It Shows:**
*   Default gateway
*   Network destinations
*   Interface mappings

---

## Network Monitoring Commands

### 1. netstat - Network Statistics

Displays active network connections, listening ports, and statistics.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `netstat -a` | Show all connections and listening ports | Saare connections dikhata hai | `netstat -a` |
| `netstat -tulnp` | TCP/UDP listening ports with process info (Linux) | Ports aur processes dikhata hai | `sudo netstat -tulnp` |

**netstat Flags Breakdown:**

| Flag | Meaning | Description |
| :--- | :--- | :--- |
| `-t` | TCP connections | TCP protocol connections |
| `-u` | UDP connections | UDP protocol connections |
| `-l` | Listening ports | Services waiting for connections |
| `-n` | Numeric addresses | Show IP addresses, not domain names |
| `-p` | Process name | Which program opened the port |

**Example Output:**
```text
Proto Local Address       Foreign Address     State       Process
tcp   0.0.0.0:22          0.0.0.0:*           LISTEN      1234/sshd
tcp   127.0.0.1:3306      0.0.0.0:*           LISTEN      5678/mysqld
udp   0.0.0.0:68          0.0.0.0:*                       1111/dhclient
```

**Understanding the Output:**

*   **0.0.0.0:22** → SSH service listening on port 22 (all interfaces)
*   **127.0.0.1:3306** → MySQL running only on localhost
*   **UDP 68 dhclient** → DHCP client waiting for IP address from router

**TCP vs UDP:**

| Feature | TCP | UDP |
| :--- | :--- | :--- |
| **Analogy** | Phone call 📞 | Loudspeaker 📢 |
| **Connection** | Established (handshake) | No connection |
| **Reliability** | Guaranteed delivery | No guarantee |
| **States** | LISTEN, ESTABLISHED, CLOSE | No states (UNCONN) |
| **Use Cases** | Web browsing, file transfer, SSH | Video streaming, DNS, gaming |

**UNCONN State (UDP):**
*   **UNCONN** = UDP service in wait mode
*   Meaning: "I'm sitting here, if a packet arrives, I'll look at it."
*   Example: **dhclient** waits for DHCP packets from router

### 2. arp - Address Resolution Protocol

Shows IP-to-MAC address mappings.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `arp -a` | Display ARP cache | IP aur MAC address mapping dikhata hai | `arp -a` |
| `arp -a -v` | Verbose ARP cache | Detailed ARP information | `arp -a -v` |

**What It Shows:**
*   Local network devices' IP and MAC addresses
*   Helps identify devices on your network

### 3. systeminfo - System Information (Windows)

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `systeminfo` | Display detailed system info | System ki complete information dikhata hai | `systeminfo` |

**Information Displayed:**
*   OS version and build
*   System manufacturer and model
*   Network card details
*   Installed hotfixes and patches

---

## File Transfer Commands

### 1. curl - Client URL Tool

A versatile tool for testing web servers and making HTTP requests.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `curl <url>` | Fetch webpage content | Webpage ka response check karta hai | `curl https://example.com` |
| `curl -I <url>` | Show only headers | Sirf headers dikhata hai | `curl -I https://example.com` |
| `curl -u user:pass <url>` | Login with credentials | Username aur password ke saath login | `curl -u user:pass http://website.com/login` |

**What curl Does:**
*   **Doctor checking patient:** Is the server alive or down?
*   Tests if:
    - Webpage is responding
    - Server is online
    - Authentication works
*   Shows:
    - HTTP status codes (200 OK, 404 Not Found, 500 Server Error)
    - Server type (nginx, Apache)
    - Cookies and headers

**Use Cases:**
*   Test API endpoints (GET, POST, PUT, DELETE requests)
*   Check server response and status codes
*   Debug authentication issues
*   View server headers and cookies

**curl vs wget:**

| Feature | curl | wget |
| :--- | :--- | :--- |
| **Purpose** | Tester/Inspector | Downloader/Collector |
| **Use Case** | Check server status | Download files |
| **Analogy** | Doctor checking patient | Ambulance bringing patient home |

### 2. wget - Website/File Getter

Downloads files from the internet with resume support.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `wget <url>` | Download file | File download karta hai | `wget https://example.com/file.iso` |
| `wget -c <url>` | Resume interrupted download | Download beech me rukne par resume karta hai | `wget -c https://example.com/bigfile.iso` |
| `wget -O <filename> <url>` | Save with custom filename | Custom naam se save karta hai | `wget -O myfile.txt https://example.com/data.txt` |

**What wget Does:**
*   Downloads large files (ISO images, software, ZIP archives)
*   Resumes downloads if interrupted
*   Creates local copies of websites
*   Better for downloading than checking

**Use Cases:**
*   Download Linux ISO images
*   Backup websites locally
*   Download software packages

---

## Advanced Networking Tools

### 1. SCP - Secure Copy Protocol

Securely transfer files between computers over SSH.

| Command | Explanation (English) | Explanation (Urdu) | Example |
| :--- | :--- | :--- | :--- |
| `scp <file> user@host:/path/` | Copy file to remote server | File ko remote system pe bhejta hai | `scp report.pdf office@192.168.1.10:/home/office/reports/` |
| `scp user@host:/path/file .` | Copy file from remote server | Remote se file download karta hai | `scp user@192.168.1.10:/home/data.txt .` |

**Example Breakdown:**
```bash
scp report.pdf office@192.168.1.10:/home/office/reports/
```

1. **scp:** Secure copy command
2. **report.pdf:** Local file to transfer
3. **office@192.168.1.10:** Remote username and IP
4. **:/home/office/reports/:** Destination path on remote server

**Real-World Example:**
You have `report.pdf` on your laptop and need to send it to office server:

```bash
scp report.pdf office@192.168.0.50:/home/office/reports/
```

File securely transfers over the network to the office server.

**SCP vs FTP:**

| Feature | SCP | FTP |
| :--- | :--- | :--- |
| **Security** | Encrypted (SSH) | Not encrypted (plain text) |
| **Speed** | Slightly slower (encryption) | Faster, but insecure |
| **Use Case** | Remote secure file transfer | Legacy systems, private LANs |

**Best Practice:** Use SCP for cloud and server management; FTP only for old systems or private networks.

---

## Understanding Network Concepts

### Network Interfaces

When you run `ifconfig` or `ip addr`, you see different network interfaces:

#### 1. lo (Loopback Interface)

*   **IP Address:** `127.0.0.1`
*   **Purpose:** Internal communication within your own computer
*   **Analogy:** Talking to yourself

**Example:**
```bash
ping 127.0.0.1
```
This pings your own system, testing if networking software is working.

#### 2. eth0 (Ethernet Interface)

*   **Purpose:** Physical network card connection
*   **Connects to:** Router, LAN, Internet
*   **Example IP:** `192.168.0.101`

This is your real network connection to the outside world.

### IP Addresses and Network Components

**Example Network Configuration:**
*   **IP Address:** `192.168.0.101` (your local/private IP)
*   **Broadcast:** `192.168.0.255` (message to all devices on `192.168.0.x` network)
*   **Subnet Mask:** `255.255.255.0` (defines network range)
*   **Default Gateway:** `192.168.0.1` (router connecting you to internet)
*   **MAC Address:** `08:00:27:76:d7:69` (unique hardware ID of network card)
*   **IPv6:** `fe80::a00:27ff:fe76:d769/64` (new style IP address, future-ready)

### IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
| :--- | :--- | :--- |
| **Address Format** | `192.168.0.101` | `fe80::a00:27ff:fe76:d769` |
| **Address Space** | ~4 billion addresses | Virtually unlimited |
| **Status** | Running out globally | Future standard |

### Interface Flags

When you see `<BROADCAST,MULTICAST,UP,LOWER_UP>`:

*   **BROADCAST:** Can send packets to all devices on network
*   **MULTICAST:** Supports multicast (group messaging)
*   **UP:** Interface is ON
*   **LOWER_UP:** Cable is connected (or wireless is connected)
*   **NO-CARRIER:** No physical connection (cable unplugged, no Wi-Fi)

### MTU (Maximum Transmission Unit)

*   **Default:** 1500 bytes
*   **Purpose:** Maximum packet size that can be sent
*   **Analogy:** Size of boxes you can ship through postal service

---

## Practical Examples

### Example 1: Check Router Connectivity

```bash
ping 192.168.0.1
```

**If successful:** Router is online and reachable.

**If failed:** Check if:
*   Cable is connected
*   Wi-Fi is enabled
*   Router is powered on

### Example 2: Test Internet Connection

```bash
ping 8.8.8.8
```

**8.8.8.8** = Google's DNS server (reliable test server)

**If successful:** Internet is working.

**If failed but router ping works:** ISP connection problem.

### Example 3: Diagnose DNS Problems

```bash
ping google.com
ping 8.8.8.8
```

**Scenario:**
*   `ping 8.8.8.8` ✅ Works
*   `ping google.com` ❌ Fails

**Diagnosis:** DNS problem (internet works, but domain name resolution is broken).

**Solution:** Change DNS servers to Google DNS:
*   Primary: `8.8.8.8`
*   Secondary: `8.8.4.4`

### Example 4: Trace Route to Destination

```bash
traceroute google.com
```

**Output:**
```text
1    192.168.0.1     1.2 ms    (Your router)
2    10.50.0.1       5.2 ms    (ISP router)
3    172.217.0.142  20.3 ms    (Google server)
```

**Analysis:**
*   Each hop shows a router your packet passes through
*   Increasing times show distance
*   High latency at specific hop indicates bottleneck

### Example 5: Check Open Ports

```bash
sudo netstat -tulnp
```

**Output:**
```text
tcp   0.0.0.0:22          LISTEN      1234/sshd
tcp   127.0.0.1:3306      LISTEN      5678/mysqld
```

**What it tells you:**
*   **Port 22 (SSH):** Open for remote connections
*   **Port 3306 (MySQL):** Running on localhost only

### Example 6: Find Open Ports and Services

```bash
sudo netstat -tulnp
```

**Use Case:** Check which services are running and which ports are listening for connections.

### Example 7: Transfer File to Remote Server

```bash
scp report.pdf admin@192.168.1.50:/home/admin/documents/
```

**What happens:**
1. Connects to remote server via SSH
2. Authenticates with password/key
3. Securely transfers `report.pdf` to destination folder

---

## Troubleshooting Common Issues

### Issue 1: "Network is Unreachable"

**Symptom:**
```bash
ping 8.8.8.8
# Output: Network is unreachable
```

**Solutions:**
1. Check if network interface is up:
   ```bash
   ip link show
   ```
2. Restart network service:
   ```bash
   sudo systemctl restart NetworkManager
   ```
3. Check default gateway:
   ```bash
   ip route
   ```

### Issue 2: DNS Resolution Failing

**Symptom:**
*   `ping google.com` fails
*   `ping 8.8.8.8` works

**Solutions:**
1. Check DNS servers:
   ```bash
   cat /etc/resolv.conf
   ```
2. Temporarily use Google DNS:
   ```bash
   echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
   ```

### Issue 3: Slow Internet Connection

**Diagnosis Steps:**

1. Check latency to router:
   ```bash
   ping -c 10 192.168.0.1
   ```

2. Check latency to internet:
   ```bash
   ping -c 10 8.8.8.8
   ```

3. Trace route to identify bottleneck:
   ```bash
   traceroute google.com
   ```

**Look for:**
*   High ping times (>100ms to router = local problem)
*   Packet loss
*   High latency at specific hops

### Issue 4: Cannot Connect to Specific Website

**Diagnosis:**

1. Test connectivity:
   ```bash
   ping website.com
   ```

2. Check DNS resolution:
   ```bash
   nslookup website.com
   ```

3. Test with curl:
   ```bash
   curl -I https://website.com
   ```

**Common causes:**
*   Website is down
*   DNS server can't resolve domain
*   Firewall blocking access

---

## Best Practices

1.  **Regular Network Checks:** Periodically run `ping` and `traceroute` to monitor network health.

2.  **Use DNS Wisely:** Always test with IP addresses first (e.g., `8.8.8.8`) before domain names to isolate DNS issues.

3.  **Document Your Network:** Keep records of:
    *   Router IP (gateway)
    *   DNS servers
    *   Static IP assignments
    *   Port configurations

4.  **Security First:** 
    *   Use SCP instead of FTP for file transfers
    *   Regularly check open ports with `netstat`
    *   Monitor unusual network connections

5.  **Understand Before Acting:** Always run diagnostic commands (`ping`, `traceroute`, `nslookup`) before making changes.

6.  **Use Appropriate Tools:**
    *   Quick checks: `ping`
    *   Detailed diagnostics: `traceroute`
    *   DNS issues: `nslookup` or `dig`
    *   Connection monitoring: `netstat`

7.  **Keep Commands Handy:** Create a reference sheet of commonly used commands for quick access during troubleshooting.

8.  **Test in Isolation:** When troubleshooting, test one thing at a time:
    *   First: Local router
    *   Then: Internet connectivity
    *   Finally: Specific services

---

## Conclusion

Networking commands are essential tools for anyone working with computers and networks. By mastering these commands, you can:

*   Quickly diagnose and resolve connectivity issues
*   Monitor network performance and security
*   Transfer files securely between systems
*   Understand how data flows across networks

**Quick Reference Cheat Sheet:**

| Task | Command |
| :--- | :--- |
| Test connectivity | `ping 8.8.8.8` |
| Check network config | `ipconfig` (Windows) / `ip addr` (Linux) |
| Lookup domain IP | `nslookup google.com` |
| Trace network path | `traceroute google.com` |
| Check open ports | `netstat -tulnp` |
| Transfer file securely | `scp file.txt user@host:/path/` |
| Test web server | `curl https://example.com` |
| Download file | `wget https://example.com/file.zip` |

**Remember:**
*   Start with basic commands (`ping`, `ipconfig`)
*   Progress to advanced tools (`dig`, `traceroute`, `netstat`)
*   Always document your findings
*   Practice regularly to build confidence

For more detailed information, consult command manual pages using `man <command>` or official documentation.

---

**Author:** Abdul Wahid  
**GitHub:** [@abdul-wahid022](https://github.com/abdul-wahid022)  
**Module:** Networking Essentials - Command Line Tools

---

*Keep Learning, Keep Networking! 🌐*