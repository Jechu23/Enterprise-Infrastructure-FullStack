# 🛡️ Firewall Policy & Traffic Enforcement

This document details the stateful inspection rules implemented in pfSense to manage traffic flow between the various security zones. The policy follows the **Principle of Least Privilege**, ensuring that only authorized traffic can traverse the network segments.

## 💻 LAN Interface Rules (Management & Core Services)

The LAN segment houses the Domain Controller and RADIUS server. Rules are configured to allow management traffic while ensuring core services can reach necessary external resources.

| Action | Protocol | Source | Port | Destination | Port | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ✅ PASS | IPv4 * | LAN net | * | * | * | Default Allow: Internal management traffic to any destination. |
| ✅ PASS | UDP | LAN net | * | 172.16.10.10 | 53, 123 | Allow internal DNS and NTP requests to the Domain Controller. |

### Configuration Details:
* **Scope:** `LAN net` encompasses the `172.16.10.0/24` management network.
* **Routing:** Outbound traffic is routed through the WAN gateway with NAT translation.

---

## 🛡️ DMZ Interface Rules (OPT1 - Isolated Zone)

The DMZ is designed to host untrusted or semi-trusted workloads. The rules below enforce strict isolation to prevent lateral movement toward the core infrastructure.

| Action | Protocol | Source | Port | Destination | Port | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ❌ BLOCK | IPv4 * | OPT1 net | * | LAN net | * | **Zero-Trust:** Prevent lateral movement from DMZ to Core LAN. |
| ✅ PASS | IPv4 * | OPT1 net | * | * | * | Allow DMZ hosts restricted internet access for updates. |

### Security Logic:
* **Micro-Segmentation:** The first rule is an explicit **Block** rule. In the event of a DMZ host compromise, this prevents an attacker from performing internal network scanning or pivoting into the Domain Controller segment.
* **Controlled Egress:** The second rule allows servers in the OPT1 zone to reach external repositories for patching, while still being restricted by the primary block rule above it.



---

## 🔑 VPN Interface Rules (Remote Access)

Rules applied to the OpenVPN tab to control what remote users can access once the tunnel is established.

| Action | Protocol | Source | Port | Destination | Port | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ✅ PASS | IPv4 * | VPN net | * | 172.16.10.10 | 53, 88, 389 | Allow VPN clients to reach AD DS for Authentication & DNS. |
| ❌ BLOCK | IPv4 * | VPN net | * | LAN net | * | Block VPN access to unauthorized management IPs. |

---
*Documentation validated for the ThomasBytes Enterprise Lab Portfolio.*