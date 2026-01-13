\# 🔐 RADIUS \& Network Policy Server (NPS) Integration



This document describes the implementation of the \*\*Network Policy Server (NPS)\*\* role, which enables the \*\*AAA (Authentication, Authorization, and Accounting)\*\* framework. This service allows pfSense to authenticate VPN users against the Active Directory database.



\## ⚙️ Role Implementation

The NPS role was installed on the dedicated server `RAD-SEC-01` (or your DC if consolidated) to act as a RADIUS server. 

\- \*\*Protocol:\*\* RADIUS (Remote Authentication Dial-In User Service).

\- \*\*UDP Ports:\*\* 1812 (Authentication), 1813 (Accounting).



\## 🛡️ RADIUS Client Configuration (The NAS)

For pfSense to communicate with NPS, it must be registered as a \*\*RADIUS Client\*\*:

\- \*\*Friendly Name:\*\* `pfSense-Gateway`

\- \*\*IP Address:\*\* `172.16.10.1`

\- \*\*Shared Secret:\*\* \[Encrypted/Hardened Passphrase]

\- \*\*Vendor:\*\* RADIUS Standard.







\## 📜 Network Access Policies (Logic Engine)

To grant access, a specific \*\*Network Policy\*\* was created with a "Zero-Trust" mindset. Access is only granted if the following conditions are met:



\### 1. Conditions

\- \*\*User Groups:\*\* User must be a member of `CORP\\VPN\_Users`.

\- \*\*NAS Port Type:\*\* Virtual (VPN).

\- \*\*Authentication Method:\*\* MS-CHAP-v2 (Encrypted password challenge).



\### 2. Constraints \& Settings

\- \*\*Idle Timeout:\*\* 30 minutes.

\- \*\*Framed-Protocol:\*\* PPP.



---



\## 🛠️ Critical Troubleshooting: The Certificate Trust

During the implementation, a \*\*TLS Handshake Error\*\* occurred between pfSense and NPS. 

\- \*\*The Issue:\*\* The RADIUS server certificate was not trusted by the OpenVPN client export.

\- \*\*The Fix:\*\* The \*\*Internal Root CA\*\* from the Domain Controller was exported and imported into the pfSense Certificate Manager. The NPS server certificate was then correctly linked to this chain, ensuring a full "Chain of Trust."







---



\## ✅ Verification \& Logging

To audit authentication attempts, we monitor the \*\*Windows Event Viewer > Custom Views > Server Roles > Network Policy and Access Services\*\*:



```powershell

\# Check for successful RADIUS authentication events in the Security Log

Get-WinEvent -LogName Security | Where-Object {$\_.Id -eq 6272} | Select-Object -First 5

