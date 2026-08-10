# 🛡️ Network Security & Protocol Analysis Lab

This repository contains practical network security experiments, protocol behavior analyses, packet inspection reports, and defensive architecture notes developed during my personal study sessions.

> ⚠️ **Disclaimer & Educational Scope:** This repository consists purely of personal study notes, laboratory observations, and research documentation recorded while following cybersecurity courses and academic training materials. It is strictly created for educational purposes and self-development, with no offensive intent, warranty, or claim of professional certification. All experiments were performed within strictly controlled, isolated virtual environment setups (VMware/VirtualBox).
---

## 📂 Laboratory Modules

### 🔹 01. Linux & Network Fundamentals
- **System Administration:** Essential Kali Linux management, file system navigation, and process controls.
- **Addressing & Identity:** MAC (Layer 2) vs. IP (Layer 3) separation, OUI fingerprinting, static vs. dynamic leases.
- **Anonymity Infrastructure:** OpenVPN tunneling, Tor onion routing mechanics, and system-wide transparent proxying (Anonsurf).

### 🔹 02. Wireless Security & 802.11 Protocols
- **Radio Frequency Spectrum:** 2.4 GHz vs. 5 GHz band propagation, channel overlap, and CSMA/CA carrier sensing.
- **Interface Modes:** Managed (Station) vs. Monitor (Promiscuous) frame capturing mechanics.
- **Reconnaissance & OSINT:** Probe request tracking, Rogue AP risks, and 802.11 management frame vulnerabilities (Deauth).
- **Cryptographic Evaluation:** WEP IV statistical computation vs. WPA/WPA2 4-Way Handshake dictionary evaluations.

### 🔹 03. MITM & Protocol Behaviors
- **Network Enumeration:** ARP scanning (Netdiscover), port state analysis (Nmap), and critical service identification.
- **Poisoning Mechanics:** Stateless ARP resolution flaws, bi-directional routing, and IP forwarding bridges.
- **Traffic Analysis:** Packet inspection via Wireshark, automated sniffing with Bettercap, and PCAP logging.
- **HTTPS & Mitigation:** HSTS Preload list barriers, SSLStrip limitations, and defensive controls (DAI, Static ARP, VPN).

---

## 🛠️ Environment & Tooling

* **Environment:** Kali Linux (Attacker VM), Windows 7/10 (Target VMs)
* **Hypervisor:** VMware Workstation (Isolated NAT/Bridged)
* **Tools:** Wireshark, Bettercap, Aircrack-ng Suite, Nmap, Netdiscover, Anonsurf
