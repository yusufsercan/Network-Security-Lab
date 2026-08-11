# 📡 02. Wireless Security Labs

This module covers wireless network interface mechanics, RF spectrum monitoring, packet capture procedures, and wireless security analysis using the Aircrack-ng suite within isolated virtualized environments.

> **Note / Disclaimer:** The contents of this module consist purely of personal study notes, laboratory observations, and quick-reference cheatsheets recorded during my cybersecurity training. They are compiled solely for self-development and personal revision, and do not claim to be authoritative academic documentation or official reference materials.

---

📋 Table of Contents
──────────────────────────────────────────────────────────────────────
1. [Wireless Interface Hardware & Virtualization Setup](#1-wireless-interface-hardware--virtualization-setup)
2. [Operating Modes: Managed vs. Monitor Mode](#2-operating-modes-managed-vs-monitor-mode)
3. [Interface Management with airmon-ng](#3-interface-management-with-airmon-ng)
4. [Passive Reconnaissance with airodump-ng](#4-passive-reconnaissance-with-airodump-ng)
5. [Wireless Encryption Protocols & Security Analysis (WEP vs. WPA/WPA2)](#5-wireless-encryption-protocols--security-analysis-wep-vs-wpawpa2)

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


