# 🛡️ RADIUS Network Policies & Access Control Logic

Network Policies are the conditional engine of the **Network Policy Server (NPS)**. They act as the final gatekeeper, evaluating connection requests against a set of predefined security criteria before granting access to the network infrastructure.

## 📋 Policy Definition: "Allow_VPN_Access"

This policy is architected to enforce strict access control for the remote entry points of the `corp.thomasbytes.ca` domain.

### 1. Decision Conditions (The "Who")
To satisfy this policy, the incoming request must meet the following mandatory attribute:
* **User Groups:** `CORP\VPN_Users`
* **Logic:** Access is strictly denied to any account—even if the credentials are valid—unless they are members of this specific security group. This ensures a specialized perimeter for VPN access.

### 2. Authentication Cryptography (The "How")
The policy is hardened to negotiate only secure challenges, preventing the use of legacy, clear-text protocols:
* **Primary Method:** **MS-CHAP-v2** (Microsoft Challenge Handshake Authentication Protocol version 2). This provides mutual authentication and two-way encryption.
* **Security Note:** **PAP**, **SPAP**, and **CHAP** have been explicitly **disabled**. By rejecting these "Less Secure Methods," we mitigate the risk of credential interception and "Pass-the-Hash" style attacks within the RADIUS exchange.

### 3. Applied Constraints & Permissions
* **Access Permission:** Set to **"Grant Access"** if conditions match.
* **Session Management:** Configured with an **Idle Timeout** of 30 minutes to prevent dormant sessions from remaining active on the gateway.
* **Framed-Protocol:** Set to **PPP** (Point-to-Point Protocol) to align with the pfSense OpenVPN tunneling requirements.

---

## ⚙️ Policy Processing Order
The NPS server is configured to evaluate policies in a top-down hierarchy. The `Allow_VPN_Access` policy is placed at **Processing Order: 1**, followed by a default **"Deny All"** catch-all policy. This follows the **Zero Trust** principle: *anything not explicitly allowed is denied.*



---

## 📸 Policy Validation
![Network Policy Overview](../screenshots/nps-policy-logic.png)

*Figure: NPS policy summary displaying the membership condition for the VPN_Users security group and the enforcement of MS-CHAP-v2.*

---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*