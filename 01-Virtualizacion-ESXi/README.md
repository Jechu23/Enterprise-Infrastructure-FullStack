# 🏗️ Module 01: Virtualization Layer (VMware ESXi 6.7.0)

This module documents the underlying hardware abstraction layer and virtual networking fabric that supports the entire enterprise infrastructure.

## 💻 Hypervisor Overview
* **Software:** VMware ESXi 6.7.0 (Build 8169922)
* **Hardware Host:** [Insert your CPU/RAM specs here, e.g., Intel Xeon / 32GB RAM]
* **Storage:** Local Datastore (SSD) for VM high-performance I/O.

---

## 🌐 Virtual Networking (vSwitch & Port Groups)
The core of this project relies on network isolation. I configured a custom **Virtual Standard Switch (vSS)** to simulate a physical corporate environment with multiple security zones.

### Port Group Configuration
| Port Group | vSwitch | VLAN ID | Associated NIC | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **WAN-Uplink** | vSwitch0 | 0 (None) | vmnic0 | Connects pfSense to the physical ISP/Home router. |
| **LAN-Core** | vSwitch1 | 0 (None) | Internal | Private segment for Domain Controllers and RADIUS. |
| **OPT-Isolated**| vSwitch1 | 10 (Tag) | Internal | Isolated segment for Workstations and Guest testing. |



---

## 🖥️ Virtual Machine Inventory
The following VMs were provisioned and optimized for this lab:

| VM Name | OS | Role | Resources (vCPU/RAM) |
| :--- | :--- | :--- | :--- |
| **PF-GW-01** | FreeBSD (pfSense) | Perimeter Firewall / Router | 2 vCPU / 2GB |
| **DC-CORP-01**| Windows Server 2016 | Active Directory / DNS / DHCP | 2 vCPU / 4GB |
| **RAD-SEC-01** | Windows Server 2016 | RADIUS (NPS) Server | 1 vCPU / 2GB |
| **WIN10-CLI** | Windows 10 Pro | Domain User Workstation | 2 vCPU / 4GB |
| **SRV-LNX-01** | Ubuntu Server | Linux Endpoint Integration | 1 vCPU / 1GB |

---

## ⚙️ ESXi Optimization & Best Practices
1. **VM Tools:** Installed on all guests to ensure driver compatibility and time synchronization with the host.
2. **Promiscuous Mode:** Disabled on all port groups to prevent packet sniffing, adhering to security best practices.
3. **Resource Reservation:** RAM reservation was applied to the Domain Controller to prevent paging during high-load authentication events.

---

## 📸 Proof of Concept
* **Networking Map:** [Link to screenshots/esxi-networking.png]
* **Resource Summary:** [Link to screenshots/esxi-resources.png]
* **Storage Latency:** [Link to screenshots/esxi-datastore.png]

---

## 🛠️ Key Learning: The "Virtual Wire"
One of the main challenges was mapping the **pfSense 3-NIC setup**. 
* **NIC 1** was mapped to the **WAN** Port Group (Access to the internet).
* **NIC 2** was mapped to the **LAN** Port Group (Internal Server farm).
* **NIC 3** was mapped to the **OPT** Port Group (Untrusted client zone).
This creates a physical-like separation where no traffic passes between zones without the Firewall's approval.