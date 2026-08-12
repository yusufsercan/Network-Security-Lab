# 03. MITM & Network Traffic Analysis

This module covers local network reconnaissance, ARP poisoning mechanics, packet inspection, and Man-in-the-Middle (MITM) attack vectors within controlled lab environments.

---

## 📝 Table of Contents
1. [Network Reconnaissance & Host Discovery](#1-network-reconnaissance--host-discovery)
2. [ARP Poisoning & Traffic Redirection Mechanics](#2-arp-poisoning--traffic-redirection-mechanics)
3. [Deep Packet Inspection & Traffic Analysis with Wireshark](#3-deep-packet-inspection--traffic-analysis-with-wireshark)
4. [Automated Sniffing & Interception with Bettercap](#4-automated-sniffing--interception-with-bettercap)
5. [Defensive Architecture & Network Mitigations](#5-defensive-architecture--network-mitigations)

---

### 1. Network Reconnaissance & Host Discovery

Before initiating any network-level analysis, accurate host discovery and service identification are essential to map the target environment without disruption.

* **Layer-2 ARP Scanning (`Netdiscover`):**
  * **Mechanism:** Netdiscover sends raw ARP requests across the local subnet to map IP-to-MAC address bindings. Since ARP operates at OSI Layer 2, it bypasses host-based firewalls that block Layer-3 ICMP (ping) requests.
  * **Commands & Options:**
    * `netdiscover -r 192.168.1.0/24` : Active scanning across the specified Class C subnet.
    * `netdiscover -p` : Passive mode; silently sniffs ARP requests without emitting packets to remain undetected.

* **Port & Service Fingerprinting (`Nmap`):**
  * **Service Version Detection:** `nmap -sV -p- 192.168.1.50` scans all 65,535 TCP ports to determine active services, banner strings, and daemon versions.
  * **OS Fingerprinting:** `nmap -O 192.168.1.50` analyzes TCP/IP stack behavior (window size, TCP options, TTL values) to identify the target operating system.

---

### 2. ARP Poisoning & Traffic Redirection Mechanics

Address Resolution Protocol (ARP) lacks identity verification, making local IPv4 networks vulnerable to cache poisoning and session redirection.

* **Stateless ARP Architecture:**
  * Operating systems accept unsolicited ARP replies and update their dynamic ARP table (`arp -a`) without verifying if a matching ARP request was sent.
  * **Poisoning Logic:** The attacker sends forged ARP responses declaring:
    * *To Target:* "The Default Gateway's IP has my MAC address."
    * *To Gateway:* "The Target's IP has my MAC address."

* **Traffic Routing & Interception Bridge:**
  * To route intermediate packets transparently between the target and the gateway without dropping client connections, IP forwarding mechanics are managed during active ARP spoofing sessions.
  * *Operational Impact:* If traffic forwarding is not properly initialized during an ARP spoofing attack, target connectivity drops completely, causing an unintended Denial of Service (DoS) rather than a successful interception session.

---

### 3. Deep Packet Inspection & Traffic Analysis with Wireshark

Wireshark allows security analysts to capture, decode, and analyze network frame structures and application payload data in real time or via PCAP files.

* **Critical Display Filter Syntax:**
  * `ip.addr == 192.168.1.100` : Filters all incoming and outgoing packets for a specific host.
  * `http.request.method == "POST"` : Isolates HTTP POST requests, commonly containing submitted form parameters or credential payloads.
  * `tcp.flags.syn == 1 and tcp.flags.ack == 0` : Filters initial TCP connection setup requests (SYN scanning or handshake initiation).
  * `frame contains "password"` : Performs byte-level string matching across all packet layers.

* **Stream Reassembly & Payload Extraction:**
  * **Follow TCP Stream:** Right-clicking any TCP packet and selecting *Follow -> TCP Stream* decodes the raw segments and reconstructs the full application-layer conversation (e.g., HTTP headers, unencrypted FTP commands, Telnet sessions).
  * **Exporting Objects:** Extracting transmitted files from packet captures via *File -> Export Objects -> HTTP/SMB*.

---

### 4. Automated Sniffing & Interception with Bettercap

Bettercap is a modern, modular framework designed to perform network attacks and monitoring tasks.

* **Module Configuration:**
  * `net.probe on` : Automatically discovers live endpoints via ARP probing.
  * `set arp.spoof.targets 192.168.1.50` : Specifies targeted IP addresses for selective poisoning.
  * `arp.spoof on` : Initiates the ARP cache poisoning process.
  * `net.sniff on` : Activates packet interception and credential parsing engine.

* **Modern Cryptographic Barriers (HSTS & TLS 1.3):**
  * **SSLStrip Limitations:** Legacy SSLStrip techniques attempt to downgrade `https://` links to `http://`. However, modern browsers enforce **HSTS (HTTP Strict Transport Security)**, which hardcodes mandatory HTTPS connections via preloaded browser lists.
  * **Impact:** Attempts to intercept HSTS-protected domains result in non-bypassable browser security warnings (`NET::ERR_CERT_AUTHORITY_INVALID`).

---

### 5. Defensive Architecture & Network Mitigations

Mitigating Layer-2 and Layer-3 traffic interception requires enforcing hardware-level switch security and cryptographic controls.

* **Dynamic ARP Inspection (DAI):**
  * Managed switches intercept all ARP requests and responses on untrusted ports.
  * Each packet is validated against a trusted **DHCP Snooping Binding Database** (which maps IP, MAC, Switch Port, and VLAN). Invalid ARP packets are dropped immediately.

* **Static ARP Bindings:**
  * For critical servers and gateways, IP-to-MAC mappings can be permanently assigned in the system registry/ARP table, ignoring dynamic ARP updates:
    * Windows command: `netsh interface ipv4 add neighbors "Ethernet" 192.168.1.1 00-11-22-33-44-55`

* **End-to-End Encryption:**
  * Enforcing HTTPS (TLS 1.3), SSHv2, and network-wide IPSec/VPN tunnels ensures that even if traffic is redirected at Layer 2, packet payloads remain cryptographically secure against passive eavesdropping.
