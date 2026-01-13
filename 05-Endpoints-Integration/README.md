# 🖥️ Module 05: Endpoints & System Integration

This module documents the final stage of the project: the integration of client operating systems into the managed infrastructure. It validates the end-to-end functionality of DNS, DHCP, Active Directory, and VPN/RADIUS services from the end-user perspective.

## 🏗️ Integrated Endpoints
The environment consists of a dual-OS strategy to demonstrate cross-platform management:

1.  **WIN10-CLI-01:** A Windows 10 Professional workstation joined to the `corp.thomasbytes.ca` domain.
2.  **LNX-DEB-01:** A Debian-based Linux instance used for network auditing and remote VPN testing.

---

## 🚀 Key Integration Milestones

### 1. Domain Join & Identity Validation
The Windows endpoint was successfully joined to the Active Directory forest. 
- **Verification:** Users can log in using their `CORP\username` credentials.
- **Policy Enforcement:** GPOs defined in Module 03 (Password complexity, RDP restrictions, and Login banners) are actively enforced on this node.

### 2. Network Services Consumption
Endpoints were moved from static configurations to **DHCP-assigned addressing** via the DC-CORP-01 server:
- **DNS:** Clients correctly resolve internal FQDNs and external domains.
- **Reverse Lookup:** Demonstrated through successful `nslookup` queries from the client to the gateway.

### 3. Secure Remote Access (VPN)
Testing of the **RADIUS-backed OpenVPN** tunnel:
- **Authentication:** External clients authenticate using AD credentials via NPS.
- **Isolation:** Verified that VPN clients can only access permitted segments according to pfSense firewall rules.

---

## 📂 Module Structure

To provide a detailed view of the integration, this folder contains the following technical references:

| File | Description |
| :--- | :--- |
| [**windows-domain-join.md**](./windows-domain-join.md) | Step-by-step of joining Win10 to AD and GPO verification. |
| [**linux-client-setup.md**](./linux-client-setup.md) | Configuration of the Linux node and network utility testing. |
| [**connectivity-tests.md**](./connectivity-tests.md) | ICMP, DNS, and HTTP benchmarks across subnets. |
| [**vpn-client-export.md**](./vpn-client-export.md) | Process of exporting and installing the OpenVPN profile with Root CA. |

---

## 📸 Final Validation Screenshot


---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*
