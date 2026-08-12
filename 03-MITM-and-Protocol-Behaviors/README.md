# 🕵️‍♂️ 03. MITM & Network Traffic Analysis

This module covers local network reconnaissance, ARP poisoning mechanics, packet inspection, and Man-in-the-Middle (MITM) attack vectors within controlled lab environments.

---

## 📝 Table of Contents
0. [Conceptual Overview & Threat Model](#0-conceptual-overview--threat-model)
1. [Network Reconnaissance & Host Discovery](#1-network-reconnaissance--host-discovery)
2. [ARP Poisoning & Traffic Redirection Mechanics](#2-arp-poisoning--traffic-redirection-mechanics)
3. [Deep Packet Inspection & Traffic Analysis with Wireshark](#3-deep-packet-inspection--traffic-analysis-with-wireshark)
4. [Traffic Interception & Protocol Analysis with Bettercap](#4-traffic-interception--protocol-analysis-with-bettercap)
5. [Defensive Architecture & Network Mitigations](#5-defensive-architecture--network-mitigations)

---

### 0. Conceptual Overview & Threat Model

Before executing any network-level analysis, understanding the core distinction between the underlying protocols, design flaws, and resulting threat states is essential.

* **Man-in-the-Middle (MITM) Core Definition:**
  * MITM is a positioning condition where an intermediary node places itself directly in the communication channel between two targets (e.g., Target Host ↔ Default Gateway).
  * Both targets believe they are communicating directly with each other, while all request and response traffic is routed through, inspected, or modified by the intermediary.

* **Protocol vs. Threat Chain:**
  * **ARP (Protocol):** A fundamental OSI Layer-2 communication protocol used to map IP addresses to physical MAC addresses (Not an attack).
  * **Design Vulnerability:** Complete lack of authentication. Devices trust and process unsolicited ARP responses without verifying matching requests.
  * **ARP Poisoning (Action):** The technique of broadcasting forged ARP packets to manipulate dynamic ARP caches across local endpoints.
  * **MITM (Result State):** The operational status achieved through poisoning, forcing network traffic to flow transparently through the intermediary interface.

---

### 1. Network Reconnaissance & Host Discovery

Before initiating any network-level analysis, accurate host discovery and service identification are essential to map the target environment without disruption.

* **Layer-2 ARP Scanning (`Netdiscover`):**
  * **Mechanism:** Netdiscover sends raw ARP requests across the local subnet to map IP-to-MAC address bindings. Since ARP operates at OSI Layer 2, it bypasses host-based firewalls that block Layer-3 ICMP (ping) requests.
  * **Commands & Options:**
    * `netdiscover -r 192.168.1.0/24` : Active scanning across the specified Class C subnet.
    * `netdiscover -p` : Passive mode; silently sniffs ARP requests without emitting packets to remain undetected.

* **Port & Service Fingerprinting (`Nmap`):**
  * **Service Version Detection:** `nmap -sV -p- 92.168.x.x` scans all 65,535 TCP ports to determine active services, banner strings, and daemon versions.
  * **OS Fingerprinting:** `nmap -O 92.168.x.x` analyzes TCP/IP stack behavior (window size, TCP options, TTL values) to identify the target operating system.

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

### 4. Traffic Interception & Protocol Analysis with Bettercap

Bettercap is a modular network inspection framework utilized to analyze real-time packet behavior, ARP cache mechanisms, and transport-layer encryption limits within isolated lab subnets.

* **Module Diagnostics & Environment Configuration:**
  * `net.probe on` : Initiates active endpoint discovery across local subnet nodes using Layer-2 ARP probes.
  * `set arp.spoof.targets <target_ip>` : Defines specified test IP endpoints within the isolated laboratory environment for traffic routing analysis.
  * `arp.spoof on` : Activates dynamic ARP table redirection to observe packet bridge behavior.
  * `net.sniff on` : Enables payload sniffing to identify unencrypted protocol leaks (e.g., HTTP POST parameters).

* **Cryptographic Security Limits (HSTS & TLS 1.3 Analysis):**
  * **SSLStrip Security Barriers:** Legacy SSLStrip methods attempt to force downgrade `https://` requests to unencrypted `http://`. Modern web security protocols enforce **HSTS (HTTP Strict Transport Security)**, which hardcodes HTTPS compliance directly into browser preloaded lists.
  * **Defensive Impact:** Attempts to intercept HSTS-enforced sessions are immediately blocked by modern user-agents, triggering non-bypassable certificates errors (`NET::ERR_CERT_AUTHORITY_INVALID`) and preserving user traffic integrity.

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
