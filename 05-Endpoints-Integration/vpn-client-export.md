\### 3. 📄 `vpn-client-export.md`


\# 🔐 VPN Client Export \& RADIUS Authentication



This document details the deployment of the OpenVPN client and the end-to-end authentication flow using Active Directory credentials.



\## 📦 Client Export Process

1\. \*\*pfSense Side:\*\* Used the `OpenVPN Client Export` utility.

2\. \*\*Remote Access Server:\*\* Linked to the OpenVPN instance on the WAN interface.

3\. \*\*Certificate Authority:\*\* The \*\*Internal Root CA\*\* was bundled into the `.ovpn` profile to ensure the chain of trust.



\## 🔑 Authentication Workflow (RADIUS)

1\. The user opens the \*\*OpenVPN GUI\*\*.

2\. Credentials entered: `CORP\\vpn\_user`.

3\. \*\*pfSense\*\* forwards the request to the \*\*NPS Server\*\* via RADIUS.

4\. \*\*NPS\*\* verifies the user is in the `VPN\_Users` AD Group.

5\. \*\*Tunnel Established:\*\* User receives an IP in the `172.16.30.0/24` range.

