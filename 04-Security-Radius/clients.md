\# 📡 RADIUS Clients Configuration



To enable centralized authentication, the pfSense firewall is configured as a RADIUS Client (NAS - Network Access Server) within the NPS console.



\## ⚙️ Device Details: pfSense Gateway

The following parameters establish the trust relationship between the Firewall and the Identity Server:



| Parameter | Value |

| :--- | :--- |

| \*\*Friendly Name\*\* | `pfSense-Gateway` |

| \*\*IP Address\*\* | `10.10.10.1` |

| \*\*Vendor\*\* | `RADIUS Standard` |

| \*\*Shared Secret\*\* | `Configured (Encrypted)` |



\## 🛡️ Security Best Practices

\* \*\*Shared Secret:\*\* A high-entropy alphanumeric string was used to encrypt the access-request packets between the client and the server.

\* \*\*Network Isolation:\*\* Communication is restricted to the internal Management VLAN/LAN to prevent interception of RADIUS traffic.



\## 📸 Configuration Screenshot

!\[RADIUS Client Setup](../screenshots/radius-client-config.png)

\*Figure: NPS console showing the registered pfSense client with its corresponding IP address.\*

