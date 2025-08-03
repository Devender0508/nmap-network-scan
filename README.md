# 🔍 Nmap Network Scan Report

## ⭐ Objective

To scan the local network using Nmap to identify live hosts, detect open ports and services, analyze potential security risks, and document the findings in a structured format.

---

## 🛠 Tools Used

* **Nmap** (v7.95)
* **Kali Linux (VMware)**

---

## 🌐 IP Address Range Scanned

```
192.168.1.0/24
```

> Found 16 live hosts out of 256 scanned IPs.

---

## 🔄 Workflow & Findings

### 🔹 Step 1: Basic Nmap TCP SYN Scan

```bash
sudo nmap -sS 192.168.1.0/24 -oN full_scan_result.txt
```

**Purpose:** Discover live hosts and open TCP ports on the local subnet.

#### 🔍 Result Summary:

| IP Address    | Open Ports                                | Services                                                    | MAC Address          | Risk Level | Notes                                       |
| ------------- | ----------------------------------------- | ----------------------------------------------------------- | -------------------- | ---------- | ------------------------------------------- |
| 192.168.1.1   | 21, 23\*, 53, 80, 139\*, 443, 445\*, 8000 | FTP, Telnet\*, DNS, HTTP, NetBIOS\*, HTTPS, SMB\*, HTTP-alt | 7C\:A9:6B\:B8:77\:E0 | ⚠️ High    | Router admin interface with legacy services |
| 192.168.1.63  | 5060 (filtered)                           | SIP                                                         | 72:3B:35:11\:C4\:C9  | ⚠️ Medium  | Likely VoIP device                          |
| 192.168.1.73  | 7070                                      | RealServer                                                  | 48:68:4A:25\:C4:43   | ⚠️ Medium  | Media streaming service                     |
| 192.168.1.79  | 7070                                      | RealServer                                                  | 3C:21:9C\:A7:58\:F0  | ⚠️ Medium  | Another streaming device                    |
| 192.168.1.94  | 135, 139, 445, 3389                       | MSRPC, NetBIOS, SMB, RDP                                    | F8:9E:94:72\:B9\:AB  | 🚨 High    | Windows host with exposed RDP + SMB         |
| 192.168.1.55  | None (filtered)                           | -                                                           | 50:5A:65\:C2:2A:41   | ✅ Low      | Shielded IoT or Smart TV device             |
| Other Devices | None                                      | -                                                           | Various              | ✅ Low      | No exposed ports, minimal risk              |

> \*Filtered ports are blocked by firewall or drop packets.

---

### 🔹 Step 2: Deep Scan on Router

```bash
sudo nmap -sV -O -p 21,23,53,80,139,443,445,8000 192.168.1.1 -oN deep_scan_router.txt
```

**Findings:**

* **FTP:** GNU Inetutils FTPd 1.9.4 (outdated)
* **DNS:** dnsmasq 2.80 (known vulnerabilities)
* **HTTP/HTTPS:** Tcpwrapped (firewalled), possibly admin interface
* **OS:** Linux 3.x–4.x kernel detected

### ⚠️ Router Risk Table

| Port    | Service | Version              | Risk Insight                |
| ------- | ------- | -------------------- | --------------------------- |
| 21      | FTP     | GNU Inetutils 1.9.4  | Plaintext, outdated         |
| 23      | Telnet  | - (filtered)         | Unencrypted login, filtered |
| 53      | DNS     | dnsmasq 2.80         | DNS rebinding risks         |
| 80/8000 | HTTP    | tcpwrapped           | Protected behind firewall   |
| 443     | HTTPS   | Unknown (ssl/https?) | Unverified certificate      |

---

### 🔹 Step 3: Vulnerability Scan on Windows Host

Initial attempt failed due to ping block:

```bash
sudo nmap --script vuln -p 445,3389 192.168.1.94 -oN vuln_win94.txt
```

> Result: "Host seems down. If it is really up, try -Pn"

Re-run using:

```bash
sudo nmap -Pn --script vuln -p 445,3389 192.168.1.94 -oN vuln_win94.txt
```

> This bypasses ping check and performs a vulnerability assessment on SMB and RDP.

---

## 🔐 Common Services & Risks

| Port | Service    | Description                                 | Risk   |
| ---- | ---------- | ------------------------------------------- | ------ |
| 21   | FTP        | Plaintext file transfer                     | High   |
| 23   | Telnet     | Unencrypted remote shell                    | High   |
| 53   | DNS        | Domain name resolution                      | Medium |
| 80   | HTTP       | Unsecured web traffic                       | Medium |
| 443  | HTTPS      | Secure web traffic                          | Low    |
| 139  | NetBIOS    | Windows file sharing                        | High   |
| 445  | SMB        | Windows file/printer sharing (vulnerable)   | High   |
| 3389 | RDP        | Remote Desktop Protocol (common exploit)    | High   |
| 7070 | RealServer | Streaming service, outdated on some devices | Medium |
| 5060 | SIP        | VoIP signaling                              | Medium |

---

## 📁 Saved Output Files

* `full_scan_result.txt` – Complete Nmap TCP SYN scan
* `deep_scan_router.txt` – Detailed scan of router


---

## 🧠 Command Reference

```bash
# Find local IP
ip a

# Basic scan
sudo nmap -sS 192.168.1.0/24 -oN full_scan_result.txt

# Deep scan
sudo nmap -sV -O -p 21,23,53,80,139,443,445,8000 192.168.1.1 -oN deep_scan_router.txt

# Vulnerability scan
sudo nmap -Pn --script vuln -p 445,3389 192.168.1.94 -oN vuln_win94.txt

# HTML report generation
sudo nmap -sS 192.168.1.0/24 -oX full.xml
xsltproc full.xml -o full_scan_result.html
```


The results provide practical insight into LAN exposure and build a foundation for future assessments and red team activities.
