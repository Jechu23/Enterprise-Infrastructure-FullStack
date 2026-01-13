\# 🌐 Network Address Translation (NAT) Configuration



This document details the NAT strategies implemented on the \*\*PF-GW-01\*\* appliance to manage outbound internet connectivity and inbound service mapping, ensuring internal IP obfuscation and security.



\## 📤 Outbound NAT (Egress Traffic)



While pfSense defaults to "Automatic Outbound NAT," this lab utilizes \*\*Hybrid Outbound NAT\*\*. This allows the system to automatically handle standard translation while permitting manual overrides for specific traffic, such as VPN data or static port requirements for gaming/VoIP.



\### Mapping Summary

| Interface | Source | Port | Destination | NAT Address | Static Port |

| :--- | :--- | :--- | :--- | :--- | :--- |

| \*\*WAN\*\* | 172.16.10.0/24 | \* | \* | WAN Address | NO |

| \*\*WAN\*\* | 172.16.20.0/24 | \* | \* | WAN Address | NO |

| \*\*WAN\*\* | 172.16.30.0/24 | \* | \* | WAN Address | YES |



\### Key Technical Implementations:

1\. \*\*IP Masquerading:\*\* All internal private IPs (RFC1918) are translated to the WAN interface's public/upstream IP. This ensures that the internal network structure is hidden from the external world.

2\. \*\*Static Port for VPN:\*\* For the VPN subnet (`172.16.30.0/24`), \*\*Static Port\*\* is enabled. This prevents pfSense from remapping source ports, which is critical for maintaining stable encrypted tunnels and avoiding "NAT Traversal" issues.



---



\## 📥 Inbound NAT (Port Forwarding)



To maintain a hardened security posture, \*\*Port Forwarding is disabled\*\* by default. However, specific mappings are defined for administrative testing:



| Service | Protocol | External Port | Internal IP | Internal Port |

| :--- | :--- | :--- | :--- | :--- |

| \*\*OpenVPN\*\* | UDP | 1194 | 127.0.0.1 (Self) | 1194 |



\*Note: The VPN service listens directly on the WAN interface, so a standard Port Forward is not required, but a WAN Firewall rule is active to allow the handshake.\*



---



\## 🛠️ NAT Optimization \& Security

\- \*\*NAT Reflection:\*\* Enabled (Pure NAT) for specific internal services to allow local clients to access internal servers using the public-facing FQDN.

\- \*\*Bogon/Private Blocking:\*\* NAT is reinforced by the "Block Private Networks" setting on the WAN interface, preventing IP spoofing and ensuring that NAT only processes legitimate outbound requests.







---



\## 📸 Verification \& Logs

\- \*\*Active Translations:\*\* Verified via `Diagnostics > States` to ensure that internal IPs are being correctly mapped to the WAN gateway during active sessions.

\- \*\*Outbound Connectivity Test:\*\*

&nbsp; ```bash

&nbsp; # Executed from SRV-LNX-01 (172.16.30.2)

&nbsp; curl -I \[https://www.google.com](https://www.google.com)

&nbsp; # Result: 200 OK (Verified NAT translation through PF-GW-01)

