# 📊 Advanced Authentication Flow & Logic Diagrams

This document provides a visual and narrative breakdown of the **AAA (Authentication, Authorization, and Accounting)** sequence implemented in this lab. By utilizing the **NAS-to-RADIUS** model, we ensure that sensitive identity data remains centralized within the Active Directory core.

## 🔄 Technical Sequence Breakdown

The following flow occurs every time a remote user or internal service attempts to authenticate via the pfSense gateway:

1.  **Request Initiation (NAS):** The `PF-GW-01` (Network Access Server) intercepts the login attempt. It encapsulates the credentials into a RADIUS Access-Request packet, hashed with the **Shared Secret**.
2.  **Transport Layer:** The request is transmitted over the internal management network via **UDP Port 1812** to the `RAD-SEC-01` (NPS) server.
3.  **Identity Validation:** The NPS server extracts the request and queries the **Active Directory Domain Controller** using a high-speed local LDAP/Kerberos look-up.
4.  **Policy Enforcement:** NPS evaluates the request against the `VPN_Access_Policy`. It verifies:
    * **Identity:** Does the user exist?
    * **Authorization:** Is the user a member of the `VPN_Users` security group?
    * **Constraints:** Is the authentication method (MS-CHAPv2) compliant?
5.  **Final Response:** * If all conditions are met: NPS returns an **Access-Accept** packet.
    * If any condition fails: NPS returns an **Access-Reject**, and the firewall drops the connection.

---

## 🖼️ Architecture Logic Diagram

The following diagram illustrates the "Trust Triangle" between the User, the Gateway, and the Identity Core:

![RADIUS Flow Diagram](../screenshots/radius-flow-diagram.png)

*Note: This diagram was architected using **diagrams.net** to reflect the exact L3/L4 communication paths within the ESXi vSwitch fabric.*

---

## 🔐 Security Benefits of this Flow
* **Credential Decoupling:** The pfSense firewall never stores or "knows" the user's AD password; it only acts as a relay.
* **Centralized Revocation:** To disable a user's VPN access, an admin only needs to remove them from the `VPN_Users` group in AD, without touching the firewall configuration.
* **Encrypted Handshake:** The use of a 128-bit Shared Secret ensures that RADIUS traffic cannot be replayed or spoofed by other devices on the LAN.

---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*
