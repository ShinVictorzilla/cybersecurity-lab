# 🔍 Nmap Port Scan - Home Router (Local Network)

## 📌 Objective
Perform a TCP port scan on a home router (TP-Link Archer C5) using Nmap to identify active services and evaluate potential security risks.

## 🛠️ Tools Used
- **Ubuntu**
- **Nmap 7.94**

## 🖥️ Target
- **IP Address:** 192.168.0.1 (Gateway)
- **Manufacturer:** TP-Link Technologies
- **MAC Address:** B0:95:75:A3:E3:B1

## 📡 Executed Command
```bash
nmap -sS 192.168.0.1
```

## 📊 Scan Results

| Port | State | Service | Risk Level |
| :--- | :--- | :--- | :--- |
| **21/tcp** | Open | FTP | Medium |
| **23/tcp** | Open | Telnet | High |
| **53/tcp** | Open | DNS | Low |
| **80/tcp** | Open | HTTP | Low |
| **139/tcp** | Open | NetBIOS | Medium |
| **445/tcp** | Open | Microsoft-DS | High |
| **1900/tcp** | Open | UPnP | Medium |

## 🧠 Security Analysis

- **Telnet (Port 23):** Unencrypted protocol. If exposed, credentials can be intercepted via plaintext traffic.
- **FTP (Port 21):** Transfers files in cleartext without encryption.
- **SMB (Port 445):** High-risk service and a common target for ransomware and network worms.
- **UPnP (Port 1900):** Can allow internal network devices to automatically open external ports without user consent.

## ✅ Recommendations

- Disable **Telnet** and **FTP** if they are not actively required.
- Evaluate whether **UPnP** is necessary and disable it if unused.
- Keep the router's firmware updated to the latest version.
- Use **SSH** (Port 22) instead of Telnet for secure administration whenever possible.

## 📚 Key Learnings

- Identifying active services and network protocols on local infrastructure.
- Performing risk assessment based on open ports and exposed services.
- Understanding the importance of hardening router configurations by disabling unnecessary services.

## 🔗 References

- [Nmap Official Documentation](https://nmap.org/book/man.html)
- [OWASP Network Security Cheat Sheet](https://cheatsheetseries.owasp.org/)

https://nmap.org/book/
https://www.iana.org/assignments/service-names-port-numbers



# 🌐 Network Reconnaissance Lab: Public Target Scan

> **Note:** This scan was performed against `scanme.nmap.org`, a designated service provided by the Nmap Security Scanner Project for authorized testing and educational purposes.

## 🎯 Target Information
- **Domain:** `scanme.nmap.org`
- **IPv4 Address:** `45.33.32.156`
- **Objective:** Map active services, evaluate open ports, and understand the basic security posture of a public-facing server.
- **Date:** October 2026

---

## 🛠️ Command Execution

To perform a standard TCP port scan (Top 1,000 ports), the following command was executed:

```bash
nmap scanme.nmap.org
```

---

## 📊 Terminal Output

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-10-17 23:50 -03
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.19s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 997 closed tcp ports (conn-refused)

PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
9929/tcp open  nping-echo
```

---

## 🧠 Technical Analysis & Findings

Based on the scan results, the host is active and responsive (`0.19s latency`). Out of 1,000 scanned ports, 997 actively refused the connection, indicating a well-configured firewall dropping or rejecting unnecessary traffic.

The following **3 ports** are exposed to the public internet:

| Port | Protocol | State | Service | Security Context |
| :--- | :--- | :--- | :--- | :--- |
| **22** | TCP | `open` | **SSH** | Secure Shell. Expected on Linux servers for remote administration. Requires strong authentication (SSH keys, no password login) to prevent brute-force attacks. |
| **80** | TCP | `open` | **HTTP** | Standard web traffic. It is unencrypted, meaning data transmitted can be intercepted. Ideally, it should redirect to HTTPS (Port 443). |
| **9929** | TCP | `open` | **nping-echo** | A specific service used by the Nmap project for packet echo testing and network troubleshooting. |

---

## 🚀 Next Steps (Further Enumeration)
- [ ] Run a Service Version scan (`nmap -sV`) to identify the exact OpenSSH and web server versions.
- [ ] Run an Aggressive scan (`nmap -A`) to grab banners, detect the exact Operating System, and execute default NSE (Nmap Scripting Engine) scripts.



# 🌐 Network Reconnaissance Lab: Service Version Detection

> **Note:** Scan performed against `scanme.nmap.org` (Authorized Nmap testing server).

## 🎯 Target Information

* **Domain:** `scanme.nmap.org`
* **IPv4 Address:** `45.33.32.156`
* **Objective:** Interrogate open ports to determine the exact service protocols, application names, and version numbers running on the target.
* **Scan Duration:** 29.38 seconds

## 🛠️ Command Execution

To perform a Service Version scan, the `-sV` flag was used:

```bash
nmap -sV scanme.nmap.org
```

## 📊 Terminal Output

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-10-18 00:20 -03
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.19s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 995 closed tcp ports (conn-refused)

PORT     STATE    SERVICE          VERSION
22/tcp   open     ssh              OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
80/tcp   open     http             Apache httpd 2.4.7 ((Ubuntu))
1461/tcp filtered ibm_wrless_lan
6901/tcp filtered jetstream
9929/tcp open     nping-echo       Nping echo
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## 🧠 Technical Analysis & Findings

By adding the `-sV` flag, Nmap successfully grabbed the service banners and identified the following software versions:

| Port | State | Service | Detected Version | Security Implication |
| :--- | :--- | :--- | :--- | :--- |
| **22** | `open` | SSH | **OpenSSH 6.6.1p1** | Knowing the exact OpenSSH version allows security professionals to search for specific CVEs. It also explicitly leaks the underlying OS (Ubuntu Linux). |
| **80** | `open` | HTTP | **Apache httpd 2.4.7** | Identifies the web server technology. Older versions may be susceptible to known exploits. |
| **1461** | `filtered`| ibm_wrless | **Unknown** | A firewall or packet filter is actively dropping probes to this port. No response was received. |
| **6901** | `filtered`| jetstream | **Unknown** | Similar to port 1461, traffic is being filtered by a network security appliance. |

**OS Leakage:** The Service Info explicitly confirmed the operating system (`OS: Linux; CPE: cpe:/o:linux:linux_kernel`) without requiring a dedicated OS detection scan (`-O`), relying solely on the information volunteered by the SSH and HTTP services.

## 🚀 Next Steps

* [ ] Run an Operating System detection scan (`sudo nmap -O scanme.nmap.org`) to map the exact kernel version.
* [ ] Cross-reference the discovered versions (OpenSSH 6.6.1p1 and Apache 2.4.7) with a vulnerability database like Exploit-DB or NIST NVD.




# 🌐 Network Reconnaissance Lab: Operating System Detection

> **Note:** Scan performed against `scanme.nmap.org` (Authorized Nmap testing server).

## 🎯 Target Information

* **Domain:** `scanme.nmap.org`

* **IPv4 Address:** `45.33.32.156`

* **Objective:** Utilize TCP/IP stack fingerprinting to identify the target's operating system, kernel version, and network distance.

* **Scan Duration:** 259.40 seconds (~4.3 minutes)

## 🛠️ Command Execution

To perform OS detection, the `-O` flag was used. This scan requires raw socket privileges, hence the use of `sudo`:

```bash
sudo nmap -O scanme.nmap.org

```

## 📊 Terminal Output

```text
Starting Nmap 7.94SVN ( https://nmap.org ) at 2026-10-18 00:45 -03
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.19s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
9929/tcp open  nping-echo
Aggressive OS guesses: Linux 5.0 (95%), Linux 5.0 - 5.4 (95%), HP P2000 G3 NAS device (93%), Linux 4.15 - 5.8 (93%), Linux 5.3 - 5.4 (93%), Linux 2.6.32 (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 20 hops

```

## 🧠 Technical Analysis & Findings

Based on the TCP/IP stack fingerprinting, the following insights were gathered:

| Finding | Details | Security Context | 
| ----- | ----- | ----- | 
| **Operating System** | **Linux (Kernel 5.x)** | Although an exact match was not found, the heuristic analysis indicates a 95% probability that the underlying OS is a modern Linux distribution. | 
| **Network Distance** | **20 hops** | The packet traversed 20 routing devices to reach the destination. Useful for network topology mapping and TTL (Time-To-Live) analysis. | 
| **Scan Limitations** | **Non-ideal conditions** | The lack of a 100% match suggests that a firewall or packet filter is dropping specific probes required for a complete OS fingerprint, a standard security practice on public-facing servers. | 

## 🏁 Conclusion of Reconnaissance Phase

By combining standard port scanning (`nmap`), service version detection (`-sV`), and OS detection (`-O`), we successfully profiled the target surface, confirming it is a Linux-based server running OpenSSH 6.6.1p1 and Apache 2.4.7 on ports 22 and 80, respectively.

