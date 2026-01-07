\# 🔐 Module 04: Centralized Security \& RADIUS Integration



This module focuses on the implementation of a centralized authentication system using \*\*pfSense\*\* as the Network Access Server (NAS) and \*\*Windows Network Policy Server (NPS)\*\* as the RADIUS server, backed by \*\*Active Directory Domain Services\*\*.



---



\## 🏗️ Architecture Overview



The security architecture follows the \*\*AAA (Authentication, Authorization, and Accounting)\*\* model:

\* \*\*Identity Store:\*\* Active Directory (AD DS) on `10.10.10.10`.

\* \*\*Policy Engine:\*\* Network Policy Server (NPS) on `10.10.10.20`.

\* \*\*Enforcement Point:\*\* pfSense Firewall on `10.10.10.1`.



---



\## 🛠️ Configuration Details



\### 1. Windows NPS (RADIUS Server)

\* \*\*RADIUS Client:\*\* Configured pfSense with a high-entropy Shared Secret.

\* \*\*Network Policy:\*\* `Allow\_VPN\_Access`

&nbsp;   \* \*\*Conditions:\*\* User group membership (`CORP\\VPN\_Users`).

&nbsp;   \* \*\*Constraints:\*\* Enabled \*\*PAP\*\* and \*\*MS-CHAPv2\*\* protocols to ensure compatibility with pfSense diagnostic tools.

\* \*\*Service Management:\*\* Used PowerShell `Restart-Service Ias` to force configuration reloads during the hardening process.



\### 2. pfSense (NAS)

\* \*\*Authentication Server:\*\* Connected to the NPS IP `10.10.10.20`.

\* \*\*NAS IP Binding:\*\* Explicitly forced to `LAN (10.10.10.1)` to maintain identity consistency across the internal network.

\* \*\*Port:\*\* UDP 1812 (Authentication).


