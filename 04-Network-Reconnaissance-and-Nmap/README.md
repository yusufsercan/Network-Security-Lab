# 04. Network Reconnaissance & Port Scanning (Nmap)

This module covers deep network enumeration mechanics, transport layer scanning techniques, timing heuristics, service version detection, and vulnerability discovery using the Network Mapper (Nmap) suite against isolated lab targets (Metasploitable2).

> **Note / Disclaimer:** The contents of this module consist purely of personal study notes, laboratory observations, and quick-reference cheatsheets recorded during my cybersecurity and engineering training. They are compiled solely for self-development and personal revision.

---

## Table of Contents
1. [Core Theory: Network Scanning & Port Mechanics](#1-core-theory-network-scanning--port-mechanics)
2. [Nmap vs. ARP Scanning (Netdiscover)](#2-nmap-vs-arp-scanning-netdiscover)
3. [Port States & Transport Layer Interpretation](#3-port-states--transport-layer-interpretation)
4. [Critical Well-Known Ports Reference](#4-critical-well-known-ports-reference)
5. [Target Specification & Syntax Rules](#5-target-specification--syntax-rules)
6. [Comprehensive Parameter & Flag Directory](#6-comprehensive-parameter--flag-directory)
7. [Firewall Evasion & Host Discovery Mechanics](#7-firewall-evasion--host-discovery-mechanics)
8. [Practical Lab Walkthrough: Metasploitable2 Target Analysis](#8-practical-lab-walkthrough-metasploitable2-target-analysis)
9. [NSE Script Analysis & Initial Exploitation Surface](#9-nse-script-analysis--initial-exploitation-surface)

---

## 1. Core Theory: Network Scanning & Port Mechanics

* **IP Address:** Identifies the host interface on the network (Layer 3 Logical Addressing).
* **Port (0–65535):** Software-defined network communication endpoints managed by the operating system kernel (Layer 4 Demultiplexing).
* **Protocol:** The standard syntax and state machine controlling the flow of data over that port.
* **Active Scanning Exposure:** Nmap transmits crafted Layer 3/4 probe packets (SYN, ACK, UDP, ICMP). Consequently, active scans leave explicit records in target system logs and stateful firewall / IDS / IPS inspection tables.

---

## 2. Nmap vs. ARP Scanning (Netdiscover)

| Feature | Netdiscover (ARP Layer) | Nmap (Layer 3 / 4 / 7) |
| :--- | :--- | :--- |
| **Scope** | Local Area Network (LAN / Layer 2 Domain) only | LAN, WAN, Internet (Routable networks) |
| **Mechanics** | Broadcasts raw ARP requests (`who-has`) | Transmits ICMP, TCP, UDP, and application payloads |
| **Output Data** | MAC Address, IP Address, Hardware Vendor | Port States, OS Stack, Service Version, CVEs |
| **Routability** | Non-routable across gateways/subnets | Fully routable across intermediate routers |

---

## 3. Port States & Transport Layer Interpretation
