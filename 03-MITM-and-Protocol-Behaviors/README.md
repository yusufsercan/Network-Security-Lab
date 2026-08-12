# 03. MITM & Network Traffic Analysis

This module covers local network reconnaissance, ARP poisoning mechanics, packet inspection, and Man-in-the-Middle (MITM) attack vectors within controlled lab environments.

---

## 📝 Table of Contents
1. Network Reconnaissance & Host Discovery
2. ARP Poisoning & Traffic Redirection Mechanics
3. Deep Packet Inspection & Traffic Analysis with Wireshark
4. Automated Sniffing & Interception with Bettercap
5. Defensive Architecture & Network Mitigations

---

### 1. Network Reconnaissance & Host Discovery
- **ARP Scanning (Netdiscover):** Performing passive and active Layer-2 ARP requests to discover alive hosts within local subnets without generating excessive noise.
- **Port & Service Analysis (Nmap):** Identifying active services, open ports, and operating system fingerprints on targets prior to traffic interception.

### 2. ARP Poisoning & Traffic Redirection Mechanics
- **Stateless ARP Flaws:** Analyzing how the ARP protocol's lack of authentication allows forged ARP responses to update target ARP tables (`arp -a`).
- **Bi-directional Routing:** Establishing a dual-poisoning bridge (Target ↔ Attacker ↔ Gateway) to route intermediate traffic without breaking target connectivity.
- **IP Forwarding:** Configuring kernel-level IP routing (`sysctl -w net.ipv4.ip_forward=1`) on the attacker machine to maintain transparent data flow.

### 3. Deep Packet Inspection & Traffic Analysis with Wireshark
- **PCAP Logging:** Capturing and isolating unencrypted network traffic (HTTP, FTP, Telnet) for payload inspection.
- **Display Filters:** Utilizing essential syntax for incident analysis:
  - `ip.addr == <target_ip>` (Isolate host traffic)
  - `http.request.method == "POST"` (Filter credentials and form data)
  - `tcp.flags.syn == 1 and tcp.flags.ack == 0` (Identify TCP SYN connection attempts)
- **Stream Reassembly:** Following TCP Streams (*Follow TCP Stream*) to reconstruct full plaintext communication sessions.

### 4. Automated Sniffing & Interception with Bettercap
- **Module Management:** Configuring `arp.spoof`, `net.sniff`, and `dns.spoof` modules within interactive sessions.
- **SSL/TLS Interception Limitations:** Analyzing the operational limits of `SSLStrip` against modern HSTS (HTTP Strict Transport Security) barriers and TLS 1.3 encryption.

### 5. Defensive Architecture & Network Mitigations
- **Dynamic ARP Inspection (DAI):** Implementing switch-level DAI to validate ARP packets against trusted DHCP Snooping bindings.
- **Static ARP Tables:** Binding critical gateway IP addresses to fixed MAC addresses on high-security endpoints.
- **Network Segmentation & Encryption:** Utilizing Virtual LANs (VLANs), VPN tunnels, and forcing HTTPS/SSH to neutralize local eavesdropping.
