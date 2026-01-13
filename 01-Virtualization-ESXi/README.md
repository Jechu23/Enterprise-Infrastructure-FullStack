# 🏗️ Module 01: Virtualization Layer (VMware ESXi 6.7.0)

This module documents the underlying hardware abstraction layer and the virtual networking fabric that supports the entire enterprise infrastructure. The goal was to create a robust, high-availability environment capable of simulating complex corporate network topologies.

## 💻 Hypervisor Overview
* **Hypervisor:** VMware ESXi 6.7.0 (Build 8169922)
* **Hardware Host:** HP Z800 Workstation | 2x Intel Xeon Processors | 96GB DDR3 ECC RAM (Adjust to your actual specs)
* **Storage Architecture:** Local Datastore backed by high-performance SSDs to ensure low-latency I/O for virtualized database and identity services.

---

## 🌐 Virtual Networking (vSwitch & Port Groups)
The architectural core of this project relies on **Layer 2 isolation**. I implemented a custom **Virtual Standard Switch (vSS)** architecture to simulate a physical corporate environment with strictly defined security zones.

### Port Group Configuration & Segmentation
| Port Group | vSwitch | VLAN ID | Associated NIC | Security Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **WAN-Uplink** | vSwitch0 | 0 (None) | vmnic0 | Public-facing uplink connecting pfSense to the physical ISP Gateway. |
| **LAN-Core** | vSwitch1 | 0 (None) | Internal | Private Management segment for Domain Controllers and RADIUS infrastructure. |
| **OPT-Isolated** | vSwitch1 | 10 (Tag) | Internal | Untrusted segment for Guest testing and workstation isolation via VLAN tagging. |

---

## 🖥️ Virtual Machine Inventory
The following instances were provisioned, hardened, and optimized for this infrastructure:

| VM Name | OS | Role | Resources (vCPU/RAM) |
| :--- | :--- | :--- | :--- |
| **PF-GW-01** | FreeBSD (pfSense 2.7.x) | Perimeter Firewall / VPN Gateway | 2 vCPU / 2GB |
| **DC-CORP-01** | Windows Server 2022 | Active Directory Domain Services / DNS | 2 vCPU / 4GB |
| **RAD-SEC-01** | Windows Server 2022 | RADIUS (NPS) Authentication Server | 1 vCPU / 2GB |
| **SRV-LNX-01** | Debian 12 (Headless) | Remote Linux Endpoint Integration | 1 vCPU / 1GB |

---

## ⚙️ Infrastructure Optimization & Security Best Practices
To ensure a production-grade environment, the following configurations were applied:

1.  **VMware Tools Integration:** Deployed on all Guest OS instances to ensure driver compatibility, optimized NIC performance (VMXNET3), and precise time synchronization via the ESXi host.
2.  **Hardened Virtual Networking:** "Promiscuous Mode" and "Forged Transmits" were explicitly **Disabled** on all port groups to prevent unauthorized packet sniffing, adhering to the principle of least privilege at the MAC layer.
3.  **Video Memory Optimization:** Increased Video RAM to **128MB** and enabled 3D acceleration for management consoles to support high-resolution remote administration.
4.  **Resource Reservations:** Applied memory reservations for the Domain Controller to ensure zero paging during peak authentication bursts.

---

## 🛠️ Critical Engineering Insight: The "Triple-NIC" Logic
A key challenge was the correct orchestration of the **pfSense 3-NIC Topology**:
* **NIC 1 (WAN):** Inbound traffic and OpenVPN listener.
* **NIC 2 (LAN):** Trusted internal traffic for the server farm.
* **NIC 3 (OPT):** A sandbox environment for untrusted clients.

This configuration enforces a **Physical-to-Virtual bridge** where no packet can traverse between zones without being inspected by the pfSense stateful firewall, simulating a real-world DMZ/Internal architecture.

---

## 📸 Proof of Concept (Internal Links)
* **Network Topology Map:** `screenshots/esxi-networking.png`
* **Hypervisor Resource Summary:** `screenshots/esxi-resources.png`
* **Datastore I/O Performance:** `screenshots/esxi-datastore.png`