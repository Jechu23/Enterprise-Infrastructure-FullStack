# 🛡️ Enterprise Hybrid Lab: Security, Identity & Network Infrastructure

[![Infrastructure](https://img.shields.io/badge/Infrastructure-Hybrid--Cloud-blue.svg)](#)
[![Security](https://img.shields.io/badge/Security-Zero--Trust-red.svg)](#)
[![Identity](https://img.shields.io/badge/Identity-Active--Directory-yellow.svg)](#)

## 📖 Project Overview
This repository documents the architectural design and deployment of a full-stack enterprise laboratory. The project simulates a production-grade environment, integrating **VMware Virtualization**, **pfSense Networking**, **Windows Server Identity Services**, and **RADIUS/AAA Security**.

The core objective was to build a secure, scalable, and audited infrastructure using a **Zero-Trust** approach, ensuring that all network segments are isolated and all access is centrally managed via Active Directory.

---

## 🏗️ Architecture & Topology
The environment is hosted on a high-performance **HP Z800 Workstation** running **ESXi**, with a multi-VLAN segmentation strategy:

* **Management LAN (172.16.10.0/24):** Core services (DC, DNS, DHCP, NPS).
* **Workstations OPT1 (172.16.20.0/24):** Isolated user segment.
* **VPN Pool (172.16.30.0/24):** Dynamic pool for remote workers.
* **WAN Interface:** Uplink to the physical network via pfSense.

---

## 🛠️ Technology Stack
| Layer | Technologies |
| :--- | :--- |
| **Virtualization** | VMware ESXi 6.7/7.0, VMXNET3, Thin Provisioning |
| **Network & Firewall** | pfSense (Router/Firewall), OpenVPN, NAT, Aliases |
| **Identity & Policy** | Active Directory DS, Group Policy (GPO), DNS, DHCP |
| **AAA Security** | Windows Network Policy Server (NPS), RADIUS Protocol |
| **Endpoints** | Windows 10 Pro (Domain-Joined), Debian Linux |

---

## 🚀 Key Implementation Highlights

### 1. Centralized Identity & AAA
Integrated **pfSense with Windows NPS** via **RADIUS**. This allows remote users to authenticate using their Active Directory credentials, ensuring that access is governed by the `VPN_Users` security group policies.

### 2. Network Hardening (Zero-Trust)
Implemented granular firewall rules using **pfSense Aliases**. Workstations in the OPT1 segment can only communicate with the Domain Controller through specific ports (DNS, Kerberos, LDAP, SMB), effectively preventing lateral movement.

### 3. Active Directory Optimization
Resolved complex DNS resolution issues by configuring **Reverse Lookup Zones** and forwarders, ensuring seamless service discovery across the forest. Enforced security baselines via GPOs (RDP Access Control, Password Complexity).

---

## 📂 Project Structure
The documentation is organized into five specialized modules:

* [**01-Virtualization-ESXi**](./01-Virtualization-ESXi/): ESXi setup and VM hardware optimization.
* [**02-Network-pfSense**](./02-Network-pfSense/): VLAN segmentation, NAT, and Gateway configuration.
* [**03-Active-Directory-Core**](./03-Active-Directory-Core/): Domain controller promotion, DNS/DHCP, and GPO.
* [**04-Security-RADIUS**](./04-Security-Radius/): NPS configuration, AAA flow, and Troubleshooting.
* [**05-Endpoints-Integration**](./05-Endpoints-Integration/): Client onboarding and connectivity validation.

---

## 🔍 Professional Skills Demonstrated
* **Network Security:** VPN tunneling, Firewalling, and RADIUS integration.
* **Infrastructure as Code (Documentation):** Clear, technical, and procedimental documentation.
* **Troubleshooting:** Diagnostic capability for L2/L3 issues and protocol handshakes.
* **System Hardening:** Implementing the Principle of Least Privilege (PoLP).

---

## 🏗️ Network Topology – Secure Lab Architecture

This lab simulates a corporate remote-access environment using VLAN segmentation,
OpenVPN, and Active Directory authentication via RADIUS.

```mermaid
flowchart TB

    VPNUser["Remote Laptop\n(OpenVPN Client)\nOutside Network"]
    Internet["🌐 Internet"]
    ISP["ISP Router"]

    Host["Physical Host\nHP Z800\nVMware ESXi"]
    pfSense["pfSense\nFirewall / VPN Gateway"]

    DC["DC-CORP-01\nAD + DNS + NPS\n172.16.10.0/24"]
    PC["Win10-CLI\nOPT1\n172.16.20.0/24"]

    VPNUser -->|"OpenVPN Tunnel"| Internet
    Internet --> ISP
    ISP -->|"WAN"| pfSense

    Host --> pfSense

    pfSense -->|"Management LAN"| DC
    pfSense -->|"OPT1"| PC

    pfSense -->|"RADIUS (UDP 1812)"| DC
    PC -->|"Identity Traffic Only"| DC



### 💡 Key Challenges Overcome

DNS Recursion Issue: Resolved a critical failure where internal clients couldn't resolve external domains by configuring proper DNS Forwarders and Reverse Lookup Zones.

RADIUS Protocol Mismatch: Diagnosed and fixed an authentication failure between pfSense and NPS by aligning encryption protocols (MS-CHAPv2).

---
**Author:** Oswaldo Jesus (Jechu23)  
**Portfolio Project:** Enterprise Infrastructure Lab 2026