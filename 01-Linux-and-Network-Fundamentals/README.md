# 🐧 01. Linux & Network Fundamentals

This module contains core system administration concepts, network architecture principles, identity management at OSI Layer 2/3, and operational privacy/anonymity mechanisms.

---

## 📋 Table of Contents
1. [Linux Administration Essentials](#1-linux-administration-essentials)
2. [Network Addressing & Identity Mechanics](#2-network-addressing--identity-mechanics)
3. [DHCP Protocol & Network Weaknesses](#3-dhcp-protocol--network-weaknesses)
4. [DNS Security & Manual Configuration](#4-dns-security--manual-configuration)
5. [Anonymity Tunnels & Dark Web Architecture](#5-anonymity-tunnels--dark-web-architecture)

---

## 1. Linux Administration Essentials

### Privilege Management & File Manipulation
- **`sudo su`:** Elevates standard user session privileges to the root superuser account.
- **`rm -rf <path>`:**
  - `-r` *(Recursive)*: Traverses directory trees to remove nested items.
  - `-f` *(Force)*: Suppresses confirmation prompts and overrides write protections.
- **`nano <filename>`:** Terminal-based text editor (`Ctrl+O` to write-out, `Ctrl+X` to exit).
- **`ls -la`:** Lists directory contents including hidden system configuration files (`.dotfiles`).
- **`passwd`:** Modifies user account authentication credentials.

### Package Management & Inspection
- **`apt search <query>`:** Queries local cache for available package repositories.
- **`history -c`:** Flushes the current shell session's command history buffer to erase local terminal trails.

---

## 2. Network Addressing & Identity Mechanics

### Layer 2 vs. Layer 3 Identity

| Attribute | MAC Address (Layer 2) | IP Address (Layer 3) |
| :--- | :--- | :--- |
| **Nature** | Physical Hardware Address (Burned-in) | Logical Network Address |
| **Scope** | Local Area Network (LAN) broadcast domain | Global / Inter-network routing (WAN) |
| **Permanence** | Fixed by manufacturer (Spoofable in RAM) | Dynamic or Static network lease |

### OUI Fingerprinting (MAC Address Structure)
A MAC address consists of 12 hexadecimal digits (6 octets). The first 3 octets constitute the **Organizationally Unique Identifier (OUI)** registered via IEEE:
- `00:0c:29` : VMware Virtual Infrastructure
- `08:00:27` : Oracle VirtualBox Environment
- `D0:03:4B` : Apple Inc. Hardware
- `00:15:5D` : Microsoft Hyper-V Virtual Machine

### MAC Spoofing Procedures
To bypass MAC Filtering controls or maintain anonymity during audit engagements:
```bash
# Automated MAC Spoofing
macchanger -r wlan0     # Assigns a randomized MAC address

# Manual Interface Reconfiguration
ifconfig wlan0 down
ifconfig wlan0 hw ether 00:11:22:33:44:55
ifconfig wlan0 up


---

## 3. DHCP Protocol & Network Weaknesses

Dynamic Host Configuration Protocol (DHCP) automatically assigns IPv4 leases, gateway pointers, and DNS resolvers within a network segment:

### Core Vulnerability Vectors
- **DHCP Starvation Attack:** An attacker floods the local switch/router with randomized MAC addresses requesting IP leases, exhausting the available DHCP pool and causing a Denial of Service (DoS) for new legitimate endpoints.
- **Rogue DHCP Server:** An unauthorized DHCP server is deployed on the network segment to respond faster than the legitimate default gateway, pushing malicious Default Gateway and DNS parameters to hijack client traffic paths.

---

## 4. DNS Security & Manual Configuration

Domain Name System (DNS) operates as the Internet's directory service, resolving human-readable domain names into logical IPv4/IPv6 addresses.

### Resolv.conf System Integration
In Linux operating systems, system-wide name resolution directives are maintained within `/etc/resolv.conf`:

```bash
# Inspect current active resolvers
cat /etc/resolv.conf

# Modify resolver configuration with root privileges
sudo nano /etc/resolv.conf

# Define reliable public secure resolvers:
nameserver 8.8.8.8    # Google Public Primary DNS
nameserver 1.1.1.1    # Cloudflare Privacy-Focused Primary DNS
