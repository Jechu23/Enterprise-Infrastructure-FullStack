
# 🔑 Module 03: Identity Management & Active Directory Domain Services (AD DS)

This module documents the deployment of the "brain" of the infrastructure. The environment is built on **Windows Server 2022**, serving as the central authority for Identity, Policy Enforcement, and Network Services (DNS/DHCP).

## 🏛️ Domain Architecture
- **Domain Name (FQDN):** `corp.thomasbytes.ca`
- **Functional Level:** Windows Server 2016 (Ensuring compatibility with advanced forest features).
- **Instance Name:** `DC-CORP-01`
- **Core Roles:** - **AD DS:** Centralized user and computer management.
  - **DNS:** Authoritative name resolution for the internal forest.
  - **DHCP:** Dynamic IP allocation for the OPT and LAN segments.

---

## 🌐 Network Services Integration

### 1. Enterprise DNS & Reverse Lookup
To support secure VPN access and system auditing, the DNS role was configured with high-integrity zones:
- **Forward Lookup Zone:** Manages records for the `corp.thomasbytes.ca` namespace.
- **Reverse Lookup Zone (Critical):** Manages the `172.16.10.x` subnet. This was manually provisioned with **PTR Records** to allow remote VPN clients to verify server identities, resolving an initial `NXDOMAIN` connectivity gap.

### 2. Managed DHCP
- **Scopes:** Configured for the **OPT-Isolated** segment to provide automated IP assignment for guest workstations while maintaining the LAN as a Static-IP management zone.

---

## 🛡️ Security Hardening & Group Policy (GPOs)
The domain is hardened using custom Group Policy Objects to enforce a baseline of security across the infrastructure:

| Policy Name | Scope | Key Settings |
| :--- | :--- | :--- |
| **Global Password Policy** | Domain-wide | 12-character minimum length, complexity enabled, 5-attempt lockout. |
| **RDP Access Control** | Servers | Restricted to members of the `IT_Admins` group only. |
| **Interactive Logon** | Workstations | Displays a legal warning banner and hides the last username. |
| **Audit Policy** | Domain Controllers | Success/Failure logging for Account Logon and Object Access (NPS logs). |



---

## 👥 Identity Structure (OU Design)
The directory is organized into a clean **Organizational Unit (OU)** structure for scalable management:
- `THOMASBYTES_CORP/`
  - `Users/` (Includes the `VPN_Users` security group).
  - `Computers/` (Managed workstations).
  - `Servers/` (Infrastructure nodes).
  - `Service_Accounts/` (Dedicated accounts for NPS/RADIUS authentication).

---

## 📸 Technical Evidence
- **Domain Dashboard:** `screenshots/ad-dashboard.png`
- **DNS Zone Validation:** `screenshots/dns-zones.png`
- **GPO Report:** `screenshots/gpo-settings.html`

---

## 🛠️ Verification Command
From the integrated Linux endpoint, identity resolution is verified through the VPN tunnel:
```bash
# Validating FQDN and PTR record synchronization
nslookup 172.16.10.10 172.16.10.10
# Expected Result: name = dc.corp.thomasbytes.