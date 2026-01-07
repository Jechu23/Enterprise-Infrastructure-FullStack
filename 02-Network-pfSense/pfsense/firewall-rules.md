\# 🛡️ Firewall Rules Configuration



\## 💻 LAN Interface Rules



| Action | Protocol | Source      | Port | Destination | Port | Description |

| :--- | :--- | :--- | :--- | :--- | :--- | :--- |

| ✅ PASS | IPv4 \* | LAN subnets | \* | \* | \* | Allow all traffic from LAN hosts to any destination |



\### Configuration Details:

\* \*\*Source:\*\* `LAN subnets` was selected to include the entire `10.10.10.0/24` network.

\* \*\*Gateway:\*\* Traffic is routed through the default WAN gateway.



## 🛡️ DMZ Interface Rules

The DMZ (Demilitarized Zone) is designed to host public-facing services while protecting the internal network. The following rules enforce isolation.

| Action | Protocol | Source      | Port | Destination | Port | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ❌ BLOCK | IPv4 * | DMZ subnets | * | LAN subnets | * | Prevent lateral movement to LAN |
| ✅ PASS  | IPv4 * | DMZ subnets | * | * | * | Allow DMZ hosts to access Internet |

### Security Logic:
* **Isolation:** The first rule explicitly denies any traffic originating from the DMZ toward the LAN. This is a critical security measure to ensure that if a DMZ server (e.g., Web Server) is compromised, the attacker cannot scan or attack the internal LAN.
* **Internet Access:** The second rule allows DMZ servers to perform updates and communicate with external services.