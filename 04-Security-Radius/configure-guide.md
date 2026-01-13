# 📖 RADIUS & NPS Step-by-Step Configuration Guide

This guide provides the procedural workflow for integrating **pfSense** with **Windows Network Policy Server (NPS)** to achieve centralized AAA (Authentication, Authorization, and Accounting).

## 🎯 Objective
To eliminate local user management on perimeter devices by delegating all authentication requests to **Active Directory Domain Services (AD DS)** via the RADIUS protocol.

---

## 🛠️ Infrastructure Environment
* **RADIUS Server (NPS):** Windows Server 2022 (`172.16.10.11`)
* **NAS/Client (Gateway):** pfSense Firewall (`172.16.10.1`)
* **Identity Store:** Active Directory (`corp.thomasbytes.ca`)
* **Transport:** RADIUS over UDP (Ports 1812: Auth, 1813: Acct)

---

## 🔧 Deployment Workflow

### Step 1: NPS Role Initialization
1.  Open **Server Manager** > **Add Roles and Features**.
2.  Select **Network Policy and Access Services**.
3.  Once installed, right-click **NPS (Local)** and select **Register Server in Active Directory** (Critical for AD attribute read permissions).

### Step 2: RADIUS Client (NAS) Registration
1.  Navigate to **RADIUS Clients and Servers > RADIUS Clients**.
2.  **New Client:**
    * **Friendly Name:** `PF-GW-01`
    * **Address:** `172.16.10.1`
    * **Shared Secret:** Generate and input a high-complexity alphanumeric string.

### Step 3: Network Policy Creation
Create a policy named `VPN_Access_Policy` with the following logic:
1.  **Conditions:** Add **User Groups** > Select `CORP\VPN_Users`.
2.  **Constraints:** Under **Authentication Methods**, enable:
    * **MS-CHAP-v2:** Encrypted challenge for secure production use.
    * **PAP/SPAP:** (Optional) For initial diagnostic testing only; disable in final production hardening.
3.  **Attributes:** Ensure Service-Type is set to `Framed`.

### Step 4: pfSense Gateway Integration
1.  Log into pfSense and navigate to **System > User Manager > Authentication Servers**.
2.  **Add New Server:**
    * **Type:** RADIUS
    * **Protocol:** PAP or MS-CHAPv2 (Must match NPS).
    * **Hostname:** `172.16.10.11`
    * **Shared Secret:** Must match the secret defined in Step 2.
3.  **Advanced:** Set **NAS IP Address** to `LAN (172.16.10.1)`. This ensures NPS recognizes the packet source as the registered client.

---

## ✅ Post-Configuration Validation
To ensure the "Chain of Trust" is operational:
1.  Go to pfSense **Diagnostics > Authentication**.
2.  Select the RADIUS server and enter valid AD credentials for a user in the `VPN_Users` group.
3.  **Success:** A green "Authenticated Successfully" banner appears.
4.  **Audit:** Check Windows **Event Viewer (Security Log)** for Event ID **6272**.

---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*