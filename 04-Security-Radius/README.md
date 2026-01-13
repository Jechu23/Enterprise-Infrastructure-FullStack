# 🔐 Module 04: Centralized Security & RADIUS Integration (AAA)

This module documents the implementation of a centralized **AAA Framework** within the lab. By integrating **pfSense** as the Network Access Server (NAS) and **Windows Network Policy Server (NPS)** as the RADIUS engine, we achieve enterprise-grade identity enforcement for all remote access.

---

## 🏗️ Security Architecture Overview

The architecture follows a distributed security model to ensure that no single point of failure exists in the authentication chain:

* **Identity Store (The Brain):** Active Directory DS on `172.16.10.10`.
* **Policy Engine (The Logic):** Network Policy Server (NPS) on `172.16.10.11`.
* **Enforcement Point (The Gatekeeper):** pfSense Firewall on `172.16.10.1`.

---

## 🛠️ Technical Implementation Details

### 1. Windows NPS (RADIUS Server) Configuration
The NPS role acts as the bridge between network requests and the Active Directory database.
* **RADIUS Client:** pfSense was registered using its LAN IP (`172.16.10.1`) and a **High-Entropy Shared Secret** to encrypt the communication between the NAS and the RADIUS server.
* **Access Policy:** `VPN_Authentication_Policy`
    * **Conditions:** Membership in the `CORP\VPN_Users` security group is strictly required.
    * **Authentication Methods:** Encrypted **MS-CHAPv2** was prioritized to prevent credential sniffing during the handshake.
* **Hardening:** Used PowerShell to manage the service lifecycle and monitor policy hit counts:
    ```powershell
    # Force reload of NPS configuration after policy updates
    Restart-Service Ias
    ```

### 2. pfSense as Network Access Server (NAS)
pfSense was configured to offload authentication from its local database to the Windows environment.
* **Authentication Server:** Linked to `172.16.10.11` via UDP Port **1812**.
* **Redundancy Logic:** Configured with a 5-second timeout and 3 retries to handle transient network congestion.
* **Diagnostic Validation:** Successful integration was verified using the pfSense `Diagnostics > Authentication` tool, confirming that Active Directory users can successfully bind to the gateway.

---

## 🔍 Verification & Integrity Checks

To ensure the security loop is closed, the following logs are monitored during every connection attempt:

### NPS Accounting Logs (Event Viewer)
We monitor **Event ID 6272** (Access Granted) and **6273** (Access Denied) to audit failed login attempts, which is critical for detecting brute-force attacks on the VPN gateway.

### pfSense System Logs
Logs under `Status > System Logs > Authentication` show the hand-off from the OpenVPN service to the RADIUS backend, proving the end-to-end flow.



---

## 🚀 Lab Final Status
With this module complete, the lab provides:
1.  **Encrypted Access:** Via OpenVPN.
2.  **Centralized Identity:** Managed in Active Directory.
3.  **Policy Enforcement:** Controlled by RADIUS (NPS).
4.  **Network Isolation:** Enforced by pfSense Firewall rules.

---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*
