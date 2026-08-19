# 📡 02. Wireless Security Labs

This module covers wireless network interface mechanics, RF spectrum monitoring, packet capture procedures, and wireless security analysis using the Aircrack-ng suite within isolated virtualized environments.

> ⚠️ **Note / Disclaimer:** The contents of this module consist purely of personal study notes, laboratory observations, and quick-reference cheatsheets recorded during my cybersecurity training. They are compiled solely for self-development and personal revision, and do not claim to be authoritative academic documentation or official reference materials.

---

📋 Table of Contents
──────────────────────────────────────────────────────────────────────
1. [Wireless Interface Hardware & Virtualization Setup](#1-wireless-interface-hardware--virtualization-setup)
2. [Operating Modes: Managed vs. Monitor Mode](#2-operating-modes-managed-vs-monitor-mode)
3. [Interface Management with airmon-ng](#3-interface-management-with-airmon-ng)
4. [Passive Reconnaissance with airodump-ng](#4-passive-reconnaissance-with-airodump-ng)
5. [Wireless Encryption Protocols & Security Analysis (WEP vs. WPA/WPA2)](#5-wireless-encryption-protocols--security-analysis-wep-vs-wpawpa2)
6. [airodump-ng Telemetry & Display Field Reference](#6-airodump-ng-telemetry--display-field-reference)

---

## 1. Wireless Interface Hardware & Virtualization Setup

When performing wireless security audits, the host network interface must support raw 802.11 frame injection and promiscuous packet capture (Monitor Mode):

* **External USB Wi-Fi Adapters**: Standard built-in network cards often lack chipset-level monitor mode support or cannot be passed directly into Virtual Machines (VMs). External USB adapters with supported chipsets (e.g., Atheros, Realtek) are passed through to the Linux VM (VMware/VirtualBox) to allow raw, direct hardware control by the guest kernel.

---

## 2. Operating Modes: Managed vs. Monitor Mode

Wireless interfaces operate under distinct operational modes depending on the intended network activity:

* **Managed Mode (Station Mode)**: 
  * The default mode for standard network communication.
  * The Wi-Fi adapter connects directly to an Access Point (AP) using standard 802.11 association and authentication handshakes.
  * The interface discards all 802.11 frames not addressed to its own MAC address.

* **Monitor Mode (RF MON / RF Monitoring)**:
  * Disconnects the interface from any associated Access Point.
  * The adapter stops acting as a standard network station and devotes all processing power to sniffing raw 802.11 radio frequencies (RF) over the air.
  * Captures all broadcast, multicast, and unicast management/control/data frames transmitted by nearby APs and connected clients within range, regardless of destination MAC address.

---

## 3. Interface Management with airmon-ng

`airmon-ng` is a utility within the Aircrack-ng suite used to audit running background processes, manage wireless interfaces, and transition adapters into Monitor Mode.

### Core Workflow Commands:

```bash
# List all detected wireless interfaces and their underlying chipsets/drivers
airmon-ng

# Identify and terminate interfering background processes (e.g., NetworkManager, wpa_supplicant)
airmon-ng check kill

# Enable Monitor Mode on the target wireless interface (e.g., wlan0 -> wlan0mon)
airmon-ng start wlan0

# Stop Monitor Mode and return interface to standard Managed Mode
airmon-ng stop wlan0mon# 02 - Wireless Security Labs

Lab notes and documentation.
```

---

## 4. Passive Reconnaissance with airodump-ng

`airodump-ng` is an 802.11 packet capture tool used to scan the RF spectrum, enumerate nearby Access Points, identify associated client stations, and log 4-Way WPA/WPA2 Handshakes.

### Key Capabilities:
* **AP Enumeration**: Displays BSSID (MAC Address), PWR (Signal Strength), Beacons, Data Frames, Channel (CH), Encryption Type (WPA2, WPA3), Cipher (CCMP, TKIP), and ESSID (Network Name).
* **Station Tracking**: Maps connected client MAC addresses (STATION) to their corresponding Access Points (BSSID).
* **Handshake Capture**: Sniffs raw 802.11 authentication frames to capture WPA/WPA2 EAPOL key exchanges for offline analysis.

### Standard Capture Syntax:

```bash
# Scan all available 2.4GHz channels for initial RF discovery
airodump-ng wlan0mon

# Targeted capture on a specific Access Point and Channel
airodump-ng --bssid <TARGET_AP_MAC> --channel <CHANNEL_NO> -w <OUTPUT_PREFIX> wlan0mon
```

---

## 5. Wireless Encryption Protocols & Security Analysis (WEP vs. WPA/WPA2)

Understanding the underlying cryptographic mechanisms and inherent attack vectors of wireless encryption protocols is critical for vulnerability assessments and secure network architecture design.

### Protocol Comparison Matrix

| Feature / Metric | WEP (Wired Equivalent Privacy) | WPA / WPA2 (Wi-Fi Protected Access) |
| :--- | :--- | :--- |
| **Security Status** | Deprecated & Critically Insecure | Current Standard (WPA2-AES / WPA3) |
| **Encryption Algorithm** | RC4 (Stream Cipher with weak 24-bit IV) | AES-CCMP / TKIP |
| **Cracking Mechanism** | **Statistical / Mathematical Analysis**: Exploits weak IV collision; no wordlist required. | **Dictionary / Brute-Force Attacks**: Requires 4-Way Handshake capture (`EAPOL`) and wordlists. |
| **Primary Vulnerability** | Short Initialization Vector (IV) reuse over high data volumes. | Weak passphrase selection (susceptible to offline dictionary attacks). |
| **Mitigation / Remedy** | Upgrade hardware to support WPA2/WPA3 immediately. | Enforce strong, high-entropy passphrases (12+ mixed characters). |

---

### Cryptographic Analysis & Attack Mechanics

#### 1. WEP (Wired Equivalent Privacy) — "Paper Lock Analogy"
* **Mechanism**: Uses the RC4 stream cipher combined with a short 24-bit Initialization Vector (IV) appended to the pre-shared key.
* **Flaw**: Due to the limited size of 24-bit IVs, a busy network rapidly reuses identical IV values. An attacker capturing a high volume of data frames (`#Data`) can analyze IV collisions using tools like `aircrack-ng` to mathematically calculate the secret key without needing to guess passwords.
* **Analogy**: WEP is like a paper vault. Regardless of how complex the key is, the structure itself is inherently weak. Attackers bypass the key entirely by tearing through the cryptographic structure.

#### 2. WPA / WPA2 (Wi-Fi Protected Access) — "Steel Safe Analogy"
* **Mechanism**: Utilizes robust encryption algorithms (AES-CCMP) alongside dynamic key management protocols (TKIP/CCMP).
* **Flaw**: The underlying encryption cannot be mathematically broken in a feasible timeframe. Instead, attacks target human error in passphrase generation via offline dictionary testing.
* **Attack Workflow**:
  1. Sniff raw 802.11 frames using `airodump-ng` until a client connects (or force re-authentication via deauth frames).
  2. Capture the **4-Way Handshake** (`EAPOL` key exchange).
  3. Perform an offline dictionary attack (`aircrack-ng` or `hashcat` with wordlists like `rockyou.txt`) by computing passphrase hashes and matching them against the captured handshake.
* **Analogy**: WPA2 is like a steel vault. The structure is impenetrable; the only entry vector is stealing the key exchange (Handshake) and testing key combinations (Wordlist) until a match is found.

---

### Security Recommendations & Hardening Checklist
* **Discontinue WEP Usage**: WEP must never be deployed in operational environments.
* **Enforce Strong Passphrase Policies**: Since WPA/WPA2 security relies entirely on key complexity, passphrases must avoid dictionary words and enforce complex, high-entropy strings (e.g., 16+ characters including symbols, numbers, and mixed-case letters).
* **Migrate to WPA3**: Where hardware permits, transition to WPA3 to leverage Simultaneous Authentication of Equals (SAE), rendering offline dictionary attacks ineffective.


  ---

## 6. airodump-ng Telemetry & Display Field Reference

When running `airodump-ng`, the interface splits into two primary display sections: **Access Points (Upper Table)** and **Client Stations (Lower Table)**. Interpreting these telemetric fields correctly is critical for effective wireless target selection and frame analysis.

### A) Access Points Section (Upper Table)

| Field | Technical Description & Field Meaning |
| :--- | :--- |
| **BSSID** | **Basic Service Set Identifier**: The physical MAC address of the Access Point's wireless radio interface. |
| **PWR** | **Signal Power Level**: Measured in dBm (e.g., `-30` indicates excellent signal strength, `-80` indicates weak/distant signal). Values closer to `0` represent stronger RF signals. |
| **Beacons** | **Beacon Frames**: Management frames broadcasted by the AP (~10 times per second) to announce its presence and capabilities. |
| **#Data** | **Captured Data Frames**: The total count of captured wireless data packets (including IVs for WEP cracking). |
| **CH** | **Channel**: The specific radio frequency channel (1–14 for 2.4GHz) on which the AP is currently transmitting. |
| **MB** | **Maximum Bandwidth / Speed**: Maximum supported data rate by the AP (e.g., `54e` indicates 54 Mbps with short preamble support). |
| **ENC** | **Encryption Protocol**: The security protocol used (`WPA2`, `WPA3`, `WEP`, or `OPN` for Open/Unencrypted networks). |
| **CIPHER** | **Cipher Suite**: The underlying cryptographic algorithm used to encrypt payload data (`CCMP`/AES, `TKIP`, `WEP40`, `WEP104`). |
| **AUTH** | **Authentication Protocol**: Key exchange method used by clients (`PSK` for Pre-Shared Key, `MGT` for 802.1X Enterprise). |
| **ESSID** | **Extended Service Set Identifier**: The human-readable network name broadcasted by the Access Point (e.g., `"Home_WiFi"`). |

---

### B) Client Stations Section (Lower Table)

| Field | Technical Description & Field Meaning |
| :--- | :--- |
| **BSSID** | The MAC address of the Access Point to which the client station is currently associated (shows `(not associated)` if roaming). |
| **STATION** | The physical MAC address of the connected client device (e.g., smartphone, laptop, IoT device). |
| **PWR** | Signal strength of the client device relative to the attacker's sniffing card. |
| **Rate** | Current transmission rate between the client station and the Access Point. |
| **Lost** | The count/percentage of dropped packets lost during transmission over the air. |
| **Frames** | Total count of data frames sent by this specific client device. |
| **PROBES** | **Probe Requests**: List of network names (ESSIDs) previously saved in the client's preferred network list that the device is actively searching for over the air. |









