# 🌐 Enterprise Network Addressing & Segmentation Plan

This document outlines the Logical Addressing Schema for the ThomasBytes Infrastructure. The network is designed using a **Zone-Based Firewall** approach to ensure strict isolation between Management, Production, and Remote Access segments.

## 🏁 Network Segmentation Matrix

### 🌎 External Zone (WAN)
* **Purpose:** External connectivity and OpenVPN service listener.
* **Subnet:** `192.168.4.0/22` (Upstream ISP Segment).
* **Gateway:** `192.168.4.1`.
* **Assignment:** DHCP Reservation via Physical MAC.

### 🏢 Core Management Zone (LAN)
* **Purpose:** High-trust segment housing Identity (AD) and AAA (RADIUS) services.
* **Subnet:** `172.16.10.0/24`.
* **Gateway:** `172.16.10.1` (pfSense Virtual Interface).
* **Primary DNS:** `172.16.10.10` (Windows Server Domain Controller).

### 🛡️ DMZ / Isolated Zone (OPT1)
* **Purpose:** Sandbox environment for untrusted workloads and guest testing.
* **Subnet:** `172.16.20.0/24`.
* **Gateway:** `172.16.20.1` (pfSense Virtual Interface).

### 🔑 Remote Access Zone (VPN)
* **Purpose:** Encrypted tunnel termination for off-site administration.
* **Subnet:** `172.16.30.0/24`.
* **Dynamic Pool:** `172.16.30.2` to `172.16.30.254`.
* **Authentication:** Managed via RADIUS (NPS).

---

## 📍 Static IP Assignments (Core Infrastructure)

| Hostname | Role | IP Address | Segment |
| :--- | :--- | :--- | :--- |
| **PF-GW-01** | pfSense Gateway | `172.16.10.1` | LAN |
| **DC-CORP-01** | Domain Controller | `172.16.10.10` | LAN |
| **RAD-SEC-01** | RADIUS (NPS) Server | `172.16.10.11` | LAN |
| **SRV-LNX-01** | Debian Management | `172.16.30.2` | VPN (Dynamic) |

---

## 🛠️ Routing Logic
* **Inter-VLAN Routing:** Handled exclusively by pfSense via internal virtual interfaces.
* **Static Routes:** Pushed to VPN clients via OpenVPN options to allow reachability of the `172.16.10.0/24` subnet.
* **Reverse Path Forwarding:** Enabled to prevent IP spoofing across segments.

---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*


