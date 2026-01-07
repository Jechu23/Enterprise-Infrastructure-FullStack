\# 🛡️ RADIUS Network Policies



Network Policies are the set of rules that determine whether a connection request is authorized to access the network.



\## 📋 Policy: Allow\_VPN\_Access

This policy is designed to authorize users belonging to the specific Active Directory security group.



\### 1. Conditions

\* \*\*User Groups:\*\* `CORP\\VPN\_Users`

\* \*\*Logic:\*\* Access is only granted if the user is a member of this security group.



\### 2. Authentication Methods

\* \*\*EAP Types:\*\* None (Simple setup).

\* \*\*Less Secure Methods:\*\* MS-CHAP-v2 (Encrypted).

\* \*Note: PAP and CHAP are disabled to prevent credential interception.\*



\### 3. Permissions

\* \*\*Access Permission:\*\* Access Granted.

\* \*\*Network Access Server:\*\* Unspecified (Standard RADIUS).



\## 📸 Policy Configuration

!\[Network Policy Overview](../screenshots/nps-policy-logic.png)

\*Figure: NPS policy summary showing the condition for VPN\_Users group.\*

