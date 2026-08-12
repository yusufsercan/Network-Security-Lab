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

### ◆ 04. Vulnerability Assessment & Exploitation
- **Targeted Scanning:** NSE script usage (`smb-vuln-ms17-010`) for precise service vulnerability identification.
- **Linux Exploitation:** Exploiting Metasploitable2 backdoor vulnerabilities (vsftpd 2.3.4) for root shell access.
- **Windows Exploitation:** MS17-010 (EternalBlue) exploitation on unpatched Windows 7 systems via SMBv1 (Port 445).
- **Post-Exploitation & Meterpreter:** System privilege verification (`NT AUTHORITY\SYSTEM`), memory-injection, hash dumping (`hashdump`), and session interactions.
- **Web Application Labs:** HTTP GET, POST, PUT, DELETE method manipulation and parameter tampering (TryHackMe HTTP/REST API challenges).

---

## 🛠️ Environment & Tooling

* **Attacker VM:** Kali Linux
* **Target VMs:** Metasploitable2 (Linux), Windows 7 SP1 (Unpatched), Windows 10/11
* **Hypervisor:** VMware Workstation Pro (Isolated Host-Only / NAT Subnets)
* **Tools & Frameworks:** Metasploit Framework (MSFconsole), Nmap, Netdiscover, Wireshark, Bettercap, GCC Compiler, Aircrack-ng Suite, Anonsurf
