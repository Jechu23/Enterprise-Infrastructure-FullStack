# 🛡️ Module 02: Network Security & Perimeter Firewall (pfSense)

This module documents the deployment and hardening of the **pfSense 2.7.x** virtual appliance. Acting as the primary Security Gateway, it manages all inter-VLAN routing, stateful firewall policies, and serves as the termination point for the Remote Access VPN.

## 🌐 Interface Configuration & Segmentation
The infrastructure follows a **Strict Segmentation** model to isolate management traffic from client workloads. By leveraging the ESXi vSwitch fabric, the firewall enforces boundaries between three distinct security zones:

| Interface | Alias | Network Segment | Purpose |
| :--- | :--- | :--- | :--- |
| **WAN** | vmx0 | DHCP (ISP/External) | Public-facing NAT gateway and OpenVPN listener. |
| **LAN** | vmx1 | 172.16.10.1/24 | **Core Infrastructure:** Domain Controllers, RADIUS, and Servers. |
| **OPT** | vmx2 | 172.16.20.1/24 | **Client Zone:** Workstations, Guest VMs, and Sandbox testing. |
| **VPN** | ovpns1 | 172.16.30.1/24 | **Remote Access:** Dynamic pool for authenticated VPN clients. |

---

## 🔒 Firewall Policy Framework
The firewall is configured with a **"Deny by Default"** posture (Implicit Deny). All traffic flows are inspected, and only explicitly authorized connections are permitted.

### 🛡️ Critical Security Rules:
1. **Active Directory & Identity:** High-priority rules allow the **172.16.10.10 (DC)** to communicate with external NTP/DNS providers, while internal clients are redirected to use the DC as their sole resolver.
2. **RADIUS Protocol (AAA):** Explicitly permitted **UDP 1812/1813** traffic from the pfSense self-interface to the RADIUS Server (172.16.10.11).
3. **Inter-VLAN Enforcement:** Traffic from the **OPT** zone to the **LAN** is restricted to essential ports only (TCP/UDP 53, 88, 135, 389, 445), preventing unauthorized lateral movement.
4. **Anti-Lockout Policy:** WebGUI administration is restricted to a dedicated Management IP within the LAN segment to prevent brute-force attempts from untrusted zones.

---

## 🛠️ Advanced Network Services

### 📡 RADIUS (NPS) Integration
* **Component:** Integrated with **Module 04 (Windows NPS)**.
* **Mechanism:** pfSense acts as a **Network Access Server (NAS)**, relaying VPN authentication requests to the RADIUS server for identity validation against Active Directory.

### 🗺️ Network Address Translation (NAT)
* **Outbound NAT:** Configured in **Hybrid Mode** to allow internal segments access to the internet while maintaining a hidden internal IP topology.
* **VPN Passthrough:** Configured to handle encrypted payloads without packet fragmentation by optimizing the MTU/MSS settings for the tunnel.

---

## 📸 Verification & Proof of Concept
* **Firewall Rules Matrix:** `screenshots/firewall-rules.png`
* **Outbound NAT Mapping:** `screenshots/nat-settings.png`
* **Services Dashboard:** `screenshots/pfsense-dashboard.png`

---

## 🚀 Connectivity Validation
Validated from a headless **Debian 12** endpoint in the **VPN** segment to ensure routing and firewall integrity:

```bash
# 1. Verify Gateway Reachability
ping -c 3 172.16.10.1

# 2. Verify Cross-Segment DNS Resolution (Active Directory)
nslookup dc.corp.thomasbytes.ca 172.16.10.10

# 3. Test Security Policy (Should be REJECTED by Firewall)
# Attempting to access unauthorized internal resource
curl -I [http://172.16.10.11:8080](http://172.16.10.11:8080)