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

### ◆ 04. Vulnerability Assessment & Proof of Concept (PoC) Analysis
- **Targeted Scanning:** Utilizing Nse scripts (`smb-vuln-ms17-010`) to identify unpatched network services and assess patch compliance.
- **Linux Service Security:** Technical analysis of legacy backdoor vulnerabilities (vsftpd 2.3.4) on isolated Metasploitable2 environments for threat modeling.
- **Windows System Assessment:** Evaluating legacy SMBv1 protocol flaws (MS17-010 / EternalBlue) on unpatched Windows 7 targets and patch verification.
- **Post-Exploitation Diagnostics:** Verifying system privilege contexts (`NT AUTHORITY\SYSTEM`), analyzing credential exposure risks, and reviewing session isolation limits.
- **Web Application Labs:** Testing HTTP request methods (GET, POST, PUT, DELETE) and parameter validation behaviors (TryHackMe HTTP/REST API challenges).

- ### ◆ 05. Reconnaissance & Network Inspection Tools
- **Nmap Framework:** Active host discovery, TCP/UDP port scanning strategies, OS fingerprinting, and NSE (Nmap Scripting Engine) utilization.
- **Wireshark Packet Analysis:** Live traffic capture, PCAP file analysis, display filter syntax (`tcp.flags`, `http.request`), and stream reassembly (Follow TCP Stream).

---

## 🛠️ Environment & Tooling

* **Attacker VM:** Kali Linux
* **Target VMs:** Metasploitable2 (Linux), Windows 7 SP1 (Unpatched), Windows 10/11
* **Hypervisor:** VMware Workstation Pro (Isolated Host-Only / NAT Subnets)
* **Tools & Frameworks:** Metasploit Framework (MSFconsole), Nmap, Netdiscover, Wireshark, Bettercap, GCC Compiler, Aircrack-ng Suite, Anonsurf
