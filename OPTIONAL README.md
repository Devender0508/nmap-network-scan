# 🦈 Wireshark Packet Scanning – Cyber Security Internship Task

This task demonstrates how **Wireshark** is used to **analyze packet-level network activity**, particularly during a **TCP SYN scan** performed using **Nmap**. It complements the Nmap reconnaissance by offering deep visibility into the actual network traffic.

---

## 🧪 Task Overview

| Parameter              | Detail                                |
|------------------------|----------------------------------------|
| 🔧 Tool Used           | Wireshark v4.x on Kali Linux           |
| 🌐 Network Interface   | eth0 (wired)                           |
| 🎯 Scan Target         | 192.168.1.0/24 (local subnet)          |
| 📊 Scan Command        | `sudo nmap -sS 192.168.1.0/24`         |
| 🧰 Objective           | Capture TCP SYN packets and observe behavior of hosts |

---

## 📂 Files Included

| File Name                      | Description                                    |
|-------------------------------|------------------------------------------------|
| `wireshark_eth01AM4A3.pcapng` | Raw packet capture taken during Nmap scan      |
| `filtered_packets.txt`        | Exported packet summary showing SYN packets    |
| `wireshark_analysis.md`       | Summary of analysis and findings               |

---

## 🧠 Why Use Wireshark?

> Nmap tells you **what is open**.  
> Wireshark tells you **how the traffic actually flows**.

### 🔍 With Wireshark, we can:
- See actual TCP packets (SYN, SYN-ACK, RST)
- View timing, size, TTL, and retransmission info
- Validate how devices respond to Nmap probes
- Detect filtered ports, closed ports, or open ports in raw packet form

---

## 🔎 Filters Used in Wireshark

| Filter                                | Purpose                               |
|---------------------------------------|----------------------------------------|
| `tcp.flags.syn == 1 and tcp.flags.ack == 0` | Only show initial SYN packets sent by Nmap |
| `tcp`                                 | Show all TCP packets                   |
| `ip.addr == 192.168.1.1`              | Focus on packets involving the router  |

---

## 🖥 Sample Scan Output (Nmap Summary)
192.168.1.1 - Ports open: 21 (ftp), 53 (dns)
192.168.1.34 - 5060/tcp filtered (SIP)
192.168.1.55 - All ports filtered
192.168.1.63 - 5060/tcp filtered (SIP)
Others - Mostly closed or filtered ports

---

## 📈 Observations from Wireshark

- TCP SYN packets were sent from scanning machine (`192.168.1.34`)
- Some hosts like `192.168.1.1` responded with **SYN-ACK** → indicates open ports
- Others sent **RST** or no response (filtered)
- Port `5060` appeared filtered on multiple hosts — possibly SIP/VoIP services
- ICMPv6, IGMPv2, and UDP broadcast packets were also observed during background traffic

---

## 🔐 Key Concepts Learned

- Packet dissection at layer 2/3/4
- TCP handshake (SYN, SYN-ACK, RST)
- Difference between open, filtered, closed ports at packet level
- Detecting services like FTP, SIP, DNS through traffic behavior

---



---

## ✅ Task Status

- [x] Packet Capture Completed  
- [x] Nmap SYN Scan Executed  
- [x] Filtered Packets Exported  
- [x] Analysis Report Created  
- [x] Pushed to GitHub ✔

---




