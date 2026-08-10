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
```
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
```
---

## 5. Anonymity Tunnels & Dark Web Architecture

### Privacy Mechanisms Comparison

| Technology | Operational Layer | Traffic Encapsulation |
| :--- | :--- | :--- |
| **Proxy** | Application Layer (HTTP/SOCKS) | Forwards browser requests through an intermediary; no system-wide encryption. |
| **VPN** | Network Layer (IPsec/OpenVPN) | Encapsulates all OS traffic inside a single point-to-point encrypted tunnel. |
| **Tor Network** | Multi-Layer Relay Circuit | Routes encrypted data packets through 3 voluntary onion nodes worldwide. |

### Tor Onion Routing Protocol Mechanics
Data packets are wrapped in multiple encryption layers (like an onion) and passed through 3 distinct nodes:
1. **Entry (Guard) Node:** Knows the client's real IP address, but cannot decrypt the final packet payload or destination.
2. **Middle Relay Node:** Acts as an isolated intermediate hop; knows neither the source client IP nor the final destination.
3. **Exit Node:** Decrypts the final encryption layer and delivers the request to the target web server or `.onion` service.

### Anonsurf Transparent Proxy System
Anonsurf enforces system-wide transparent proxying through the Tor overlay network by manipulating `iptables` rules:

```bash
# Repository Installation Protocol
git clone [https://github.com/Und3rf10w/kali-anonsurf.git](https://github.com/Und3rf10w/kali-anonsurf.git)
cd kali-anonsurf
apt install secure-delete                   # Secure file erasure utility
dpkg -i --force-depends kali-anonsurf.deb  # Force package installation
apt install -f                              # Resolve broken dependencies

# Operational Commands
anonsurf start    # Enables Tor tunnel, closes IPv6 leaks, and spoofs DNS
anonsurf myip     # Queries and verifies current external exit IP & country
anonsurf status   # Verifies active Tor service execution
anonsurf stop     # Restores native iptables routing rules & network settings
```
---

---

## 6. HTTP Protocol & Web Architecture Fundamentals

### What is HTTP & HTTPS?
Web applications rely on standardized client-server protocols to transmit resources (HTML documents, images, scripts, videos):

* **HTTP (HyperText Transfer Protocol)**: Developed between 1989–1991 by Tim Berners-Lee and his team. Defines the set of rules used for communicating with web servers to transfer webpage data (HTML, Images, Videos, etc.). Transmits data in unencrypted plain text.
* **HTTPS (HyperText Transfer Protocol Secure)**: The secure version of HTTP. Encrypts transmitted data to prevent unauthorized interception/eavesdropping and provides cryptographic assurance that communication is established with the legitimate web server.

---

### What is a URL? (Uniform Resource Locator)
A URL is an instruction specifying how and where to access a specific resource on the Internet:

  Scheme      User:Password        Host/Domain     Port    Path       Query String  Fragment
  ───────  ───────────────────────  ──────────────  ────  ───────────  ────────────  ────────
  http://  user:password@           tryhackme.com   :80   /view-room   ?id=1         #task3

* **Scheme**: Specifies the communication protocol used to access the resource (e.g., HTTP, HTTPS, FTP).
* **User**: Optional basic authentication credentials embedded directly within the URL.
* **Host**: The domain name or IP address of the target web server.
* **Port**: The network port for connection (Defaults: 80 for HTTP, 443 for HTTPS; custom range: 1–65535).
* **Path**: The file name or location of the resource on the target web server.
* **Query String**: Extra parameters sent to the requested path following `?` (e.g., `?id=1`).
* **Fragment**: Client-side anchor following `#` pointing directly to a specific section within the loaded page.

---

### HTTP Requests and Responses Breakdown

#### Basic HTTP Request Anatomy
A basic HTTP request contains the Request Method, Target Path, Protocol Version, and Headers:

GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: [https://tryhackme.com/](https://tryhackme.com/)
(empty line)

* **Line 1**: Sends a `GET` request for the root homepage path `/` using protocol version `HTTP/1.1`.
* **Line 2 (`Host`)**: Specifies the target domain (`tryhackme.com`).
* **Line 3 (`User-Agent`)**: Identifies the client browser software (`Firefox v87`).
* **Line 4 (`Referer`)**: Specifies the referring web page URL that directed the browser to this resource.
* **Line 5 (Blank Line)**: A mandated empty line signaling the end of the HTTP request headers.

#### Basic HTTP Response Anatomy
The web server processes the request and responds with a Status Line, Headers, and Content Body:

HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:14:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
    <title>TryHackMe</title>
</head>
<body>
    Welcome To TryHackMe.com
</body>
</html>

* **Line 1**: Confirms `HTTP/1.1` protocol usage and returns the HTTP status code `200 OK` (successful execution).
* **Line 2 (`Server`)**: Identifies the server software name and version (`nginx/1.15.8`).
* **Line 3 (`Date`)**: Server timestamp of the response execution.
* **Line 4 (`Content-Type`)**: Specifies the format of the returned payload (`text/html`, `images`, `pdf`, `XML`, etc.).
* **Line 5 (`Content-Length`)**: Specifies the byte length of the response payload to ensure no data is missing.
* **Line 6 (Blank Line)**: A mandated empty line signaling the end of HTTP response headers.
* **Lines 7–14**: The requested HTML payload displayed in the client browser.



