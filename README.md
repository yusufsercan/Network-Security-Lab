# 🛡️ Network Security & Protocol Analysis Lab

This repository contains practical network security experiments, protocol behavior analyses, packet inspection reports, and defensive architecture notes developed during my personal study sessions.

> ⚠️ **Disclaimer & Educational Scope:** This repository consists purely of personal study notes, laboratory observations, and research documentation recorded while following cybersecurity courses and academic training materials. It is strictly created for educational purposes and self-development, with no offensive intent, warranty, or claim of professional certification. All experiments were performed within strictly controlled, isolated virtual environment setups (VMware/VirtualBox).
---

## 📂 Laboratory Modules

### ◆ 01. Linux & Network Fundamentals
- **System Administration:** Essential Kali Linux management, file system navigation, permissions, and process controls.
- **Addressing & Identity:** MAC (Layer 2) vs. IP (Layer 3) separation, OUI fingerprinting, static vs. dynamic leases.
- **Anonymity Infrastructure:** OpenVPN tunneling, Tor onion routing mechanics, and system-wide transparent proxying (Anonsurf).
- **Socket & Web Mechanics:** C/Python socket programming architecture, HTTP/HTTPS request/response lifecycles, and REST API behavior.

### ◆ 02. Wireless Security & 802.11 Protocols
- **Radio Frequency Spectrum:** 2.4 GHz vs. 5 GHz band propagation, channel overlap, and CSMA/CA carrier sensing.
- **Interface Modes:** Managed (Station) vs. Monitor (Promiscuous) frame capturing mechanics.
- **Reconnaissance & OSINT:** Probe request tracking, Rogue AP risks, and 802.11 management frame vulnerabilities (Deauth).
- **Cryptographic Evaluation:** WEP IV statistical computation vs. WPA/WPA2 4-Way Handshake dictionary evaluations.

### ◆ 03. MITM & Protocol Behaviors
- **Network Enumeration:** ARP scanning (Netdiscover), port state analysis (Nmap), and critical service identification.
- **Poisoning Mechanics:** Stateless ARP resolution flaws, bi-directional routing, and IP forwarding bridges.
- **Traffic Analysis:** Packet inspection via Wireshark, automated sniffing with Bettercap, and PCAP logging.
- **HTTPS & Mitigation:** HSTS Preload list barriers, SSLStrip limitations, and defensive controls (DAI, Static ARP, VPN).

### ◆ 04. Network Reconnaissance & Port Scanning (Nmap)
- **Architecture & Mechanics:** Host discovery lifecycles, TCP stateful flag behaviors (SYN vs. Connect), and UDP inspection constraints.
- **Scanning Methodologies:** Full spectrum auditing (-p-), timing template tuning (T0–T5), evasion heuristics (-Pn, -n, fragmented packets), and target parsing.
- **Service & OS Fingerprinting:** Banner grabbing, application version extraction (-sV), TCP/IP stack fingerprinting (-O), and aggressive scanning (-A).
- **Nmap Scripting Engine (NSE):** Automated Lua-based vulnerability detection (`--script vuln`), anonymous credential testing, and plaintext protocol leak analysis.
- **Target Environment Analysis:** Practical exploitation reconnaissance against Metasploitable2 (FTP vsftpd 2.3.4 backdoor surface mapping and plain-text exposure).

---

## 🛠️ Environment & Tooling

* **Attacker VM:** Kali Linux
* **Target VMs:** Metasploitable2 (Linux), Windows 7 SP1 (Unpatched), Windows 10/11
* **Hypervisor:** VMware Workstation Pro (Isolated Host-Only / NAT Subnets)
* **Tools & Frameworks:** Metasploit Framework (MSFconsole), Nmap, Netdiscover, Wireshark, Bettercap, GCC Compiler, Aircrack-ng Suite, Anonsurf
