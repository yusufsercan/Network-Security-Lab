# 04. Network Reconnaissance & Port Scanning (Nmap)

This module covers deep network enumeration mechanics, transport layer scanning techniques, timing heuristics, service version detection, and vulnerability discovery using the Network Mapper (Nmap) suite against isolated lab targets (Metasploitable2).

> **Note / Disclaimer:** The contents of this module consist purely of personal study notes, laboratory observations, and quick-reference cheatsheets recorded during my cybersecurity and engineering training. They are compiled solely for self-development and personal revision.

---

## Table of Contents
1. [Core Theory: Network Scanning & Port Mechanics](#1-core-theory-network-scanning--port-mechanics)
2. [Nmap vs. ARP Scanning (Netdiscover)](#2-nmap-vs-arp-scanning-netdiscover)
3. [Port States & Transport Layer Interpretation](#3-port-states--transport-layer-interpretation)
4. [Critical Well-Known Ports Reference](#4-critical-well-known-ports-reference)
5. [Target Specification & Syntax Rules](#5-target-specification--syntax-rules)
6. [Comprehensive Parameter & Flag Directory](#6-comprehensive-parameter--flag-directory)
7. [Firewall Evasion & Host Discovery Mechanics](#7-firewall-evasion--host-discovery-mechanics)
8. [Practical Lab Walkthrough: Metasploitable2 Target Analysis](#8-practical-lab-walkthrough-metasploitable2-target-analysis)
9. [NSE Script Analysis & Initial Exploitation Surface](#9-nse-script-analysis--initial-exploitation-surface)

---

## 1. Core Theory: Network Scanning & Port Mechanics

* **IP Address:** Identifies the host interface on the network (Layer 3 Logical Addressing).
* **Port (0–65535):** Software-defined network communication endpoints managed by the operating system kernel (Layer 4 Demultiplexing).
* **Protocol:** The standard syntax and state machine controlling the flow of data over that port.
* **Active Scanning Exposure:** Nmap transmits crafted Layer 3/4 probe packets (SYN, ACK, UDP, ICMP). Consequently, active scans leave explicit records in target system logs and stateful firewall / IDS / IPS inspection tables.

---

## 2. Nmap vs. ARP Scanning (Netdiscover)

| Feature | Netdiscover (ARP Layer) | Nmap (Layer 3 / 4 / 7) |
| :--- | :--- | :--- |
| **Scope** | Local Area Network (LAN / Layer 2 Domain) only | LAN, WAN, Internet (Routable networks) |
| **Mechanics** | Broadcasts raw ARP requests (`who-has`) | Transmits ICMP, TCP, UDP, and application payloads |
| **Output Data** | MAC Address, IP Address, Hardware Vendor | Port States, OS Stack, Service Version, CVEs |
| **Routability** | Non-routable across gateways/subnets | Fully routable across intermediate routers |

---

## 3. Port States & Transport Layer Interpretation

| Port State | Network Behavior |
| :--- | :--- |
| **`open`** | Application is actively listening and accepting connections. |
| **`closed`** | Host responds with RST/ACK; port is reachable but no service is bound. |
| **`filtered`** | Firewall / packet filter drops probe; state cannot be determined. |
| **`open\|filtered`** | No response received on UDP or special flag probes (Xmas, FIN, NULL). |



  ---

## 4. Critical Well-Known Ports Reference

| Port Number | Default Service | Protocol Type | Security Risk & Inspection Focus |
| :--- | :--- | :--- | :--- |
| **20 / 21** | FTP | Plaintext | Unencrypted file transfer; plaintext creds, anonymous login risk. |
| **22** | SSH | Encrypted | Secure remote administration; public key/password authentication. |
| **23** | Telnet | Plaintext | **High Risk:** Transmits credentials and shell data in cleartext. |
| **25** | SMTP | Plaintext / STARTTLS | Mail delivery; user enumeration vectors (`VRFY`, `EXPN`, `RCPT TO`). |
| **53** | DNS | UDP / TCP | Domain resolution; DNS zone transfer (`AXFR`) exposure. |
| **67 / 68** | DHCP | UDP | Dynamic network configuration; rogue DHCP / starvation attacks. |
| **80** | HTTP | Plaintext | Web service; vulnerable to sniffing and web app attack vectors. |
| **110** | POP3 | Plaintext | Post office mail retrieval; plaintext credentials. |
| **137–139** | NetBIOS | UDP / TCP | Windows legacy name resolution and session management. |
| **143** | IMAP | Plaintext | Mail synchronization; unencrypted credential exchange. |
| **389 / 636** | LDAP / LDAPS | Plaintext / SSL | Directory access; user and domain structure queries. |
| **443** | HTTPS | Encrypted (TLS) | Encrypted web communication; certificate inspections. |
| **445** | SMB (microsoft-ds) | TCP | Direct Windows file/resource sharing; high-profile CVE targets. |
| **989 / 990** | FTPS | Encrypted (TLS) | Secure FTP control and data channels. |

---

## 5. Target Specification & Syntax Rules

Nmap is flag-order agnostic (parameters are parsed before execution), but arguments requiring values must stay paired:

```bash
# Valid syntax patterns (Order does not alter execution):
nmap -p- -sV -T4 192.168.1.100
nmap -sV -T4 -p- 192.168.1.100

# Correct parameter binding:
nmap -p 80,443 -sV 192.168.1.100      # Port arguments directly follow -p
nmap -iL targets.txt -T4               # Target file directly follows -iL

# Target Definition Patterns:
nmap 192.168.1.1 192.168.1.50          # Specific host list
nmap 192.168.1.1-254                   # IP Range sweep
nmap 192.168.1.0/24                    # Subnet CIDR block
nmap -iL targets.txt                   # Input from target file
nmap 192.168.1.0/24 --exclude 192.168.1.1 # Subnet scan excluding gateway
```
