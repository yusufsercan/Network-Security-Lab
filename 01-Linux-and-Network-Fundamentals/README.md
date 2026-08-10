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

## 6. HTTP, HTTPS Protocol & Web Architecture Fundamentals

### What is HTTP & HTTPS?
* **HTTP (HyperText Transfer Protocol - Port 80)**: Standard protocol used for communicating with web servers to transfer webpage assets (HTML, images, scripts). Transmits data in unencrypted plain text.
* **HTTPS (HyperText Transfer Protocol Secure - Port 443)**: Encrypted variant of HTTP. Uses TLS/SSL encryption to secure data in transit and ensure cryptographic server authentication.

> **Note:** Status codes, request methods, headers, and cookie mechanics work identically on both HTTP and HTTPS. The only difference is that HTTPS encrypts the payload within a TLS tunnel.

---

### HTTP Request Methods
* **GET**: Retrieves data or web pages from the server without modifying state.
* **POST**: Submits data to the server to create new records (e.g., login, registration forms).
* **PUT**: Updates or replaces an existing resource on the target server.
* **DELETE**: Requests the removal of a specific record/resource from the server.

---

### HTTP Status Codes

#### Code Ranges
| Range | Category | Description |
| :--- | :--- | :--- |
| **100–199** | Informational | Process continuing. |
| **200–299** | Success | Request successfully received and fulfilled. |
| **300–399** | Redirection | Further action needed to complete request. |
| **400–499** | Client Error | Faulty request syntax or unauthorized client access. |
| **500–599** | Server Error | Exception or failure on the server side. |

#### Critical Status Codes
* `200 OK`: Request succeeded.
* `201 Created`: Resource successfully created via POST/PUT.
* `301 Moved Permanently`: Target URL permanently relocated.
* `302 Found`: Temporary redirection to a different URL.
* `400 Bad Request`: Malformed request syntax.
* `401 Unauthorized`: Missing or invalid authentication credentials.
* `403 Forbidden`: Authenticated client lacks permission for the resource.
* `404 Not Found`: Requested resource does not exist on the server.
* `405 Method Not Allowed`: HTTP method blocked for the target endpoint.
* `500 Internal Server Error`: Server encountered an unexpected exception.
* `503 Service Unavailable`: Server down due to overload or maintenance.

---

### HTTP Headers & Session Management

#### Core Headers
* **Host** *(Request)*: Specifies the target domain name.
* **User-Agent** *(Request)*: Identifies the client browser and OS parameters.
* **Content-Type** *(Response)*: Declares the format of the payload (`text/html`, `application/json`).
* **Set-Cookie** *(Response)*: Server instructs the browser to store a session token.
* **Cookie** *(Request)*: Client sends stored session tokens back to the server to maintain state.

#### Statelessness & Cookies
HTTP is inherently **stateless** (each request is executed independently). Servers issue session tokens via `Set-Cookie` upon login, and the browser automatically attaches this token in subsequent requests using the `Cookie` header to keep the session alive.




