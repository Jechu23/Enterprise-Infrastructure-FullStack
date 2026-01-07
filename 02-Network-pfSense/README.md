# 🛡️ Module 02: Network Security & Perimeter Firewall (pfSense)

This module documents the configuration of the **pfSense 2.x** appliance, acting as the primary Firewall, Router, and Gateway for the entire enterprise lab.

## 🌐 Interface Configuration & Segmentation
To enforce the principle of least privilege at the network level, the infrastructure is divided into three distinct segments:

| Interface | Network Segment | Purpose |
| :--- | :--- | :--- |
| **WAN** | DHCP (ISP/External) | External connectivity and NAT gateway. |
| **LAN** | `10.10.10.1/24` | Core Infrastructure (Domain Controllers, RADIUS, Servers). |
| **OPT** | `10.10.20.1/24` | Client Zone (Workstations, Guest VMs, Testing). |



---

## 🔒 Firewall Policy Framework
The firewall is configured with a **"Deny by Default"** posture. Traffic is only permitted through explicit rules.

### Key Rules Implemented:
1. **DNS/NTP Redirection:** Only the Domain Controller (`10.10.10.10`) is allowed to communicate with external DNS/NTP servers. All other internal traffic is intercepted.
2. **Inter-VLAN Routing:** Traffic from the **OPT** (Client) zone to the **LAN** (Server) zone is restricted to essential services only (AD Ports, DNS, DHCP).
3. **Anti-Lockout:** Secure management access to the pfSense WebGUI is restricted to specific IT Admin IPs within the LAN.

---

## 🛠️ Advanced Services

### 📡 RADIUS Integration
* **Service:** Integrated with **Module 04 (NPS)**.
* **Function:** pfSense acts as a RADIUS Client to authenticate administrative logins via Active Directory credentials.

### 🗺️ Network Address Translation (NAT)
* **Outbound NAT:** Configured to allow internal segments to access the Internet while masking internal IP schemes.
* **Port Forwarding:** (Optional) Configured for specific services if external access is required for testing.

---

## 📸 Configuration Highlights
* **Rules Overview:** [Link to screenshots/firewall-rules.png]
* **NAT Setup:** [Link to screenshots/nat-settings.png]
* **Dashboard Status:** [Link to screenshots/pfsense-dashboard.png]

---

## 🚀 Verification Commands
From a client in the **OPT** zone, the following tests were performed to verify firewall integrity:
```bash
# Verify connectivity to Gateway
ping 10.10.20.1

# Verify DNS resolution through Domain Controller
nslookup google.com 10.10.10.10

# Test blocked Inter-VLAN access (Should Fail)
ping 10.10.10.11

