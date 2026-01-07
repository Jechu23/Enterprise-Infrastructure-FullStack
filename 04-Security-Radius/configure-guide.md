\# RADIUS \& NPS Configuration Guide



\## 🎯 Objective

Centralize user authentication for network perimeter devices (pfSense) using Active Directory Domain Services.



\## 🛠️ Environment Details

\* \*\*RADIUS Server (NPS):\*\* Windows Server 2016 (`10.10.10.20`)

\* \*\*NAS/Client:\*\* pfSense Firewall (`10.10.10.1`)

\* \*\*Domain:\*\* `corp.thomasbytes`

\* \*\*Protocol:\*\* RADIUS over UDP (Ports 1812, 1813)



\## 🔧 Step-by-Step Setup

1\. \*\*NPS Installation:\*\* Installed 'Network Policy and Access Services' role via Server Manager.

2\. \*\*RADIUS Client:\*\* Registered pfSense as a client using its LAN IP and a high-complexity Shared Secret.

3\. \*\*Network Policy:\*\* Created a policy named `Allow\_VPN\_Access`:

&nbsp;  - \*\*Condition:\*\* User must belong to `CORP\\VPN\_Users`.

&nbsp;  - \*\*Authentication:\*\* Enabled \*\*PAP\*\* and \*\*MS-CHAPv2\*\*.

4\. \*\*pfSense Integration:\*\* - Configured `System > User Manager > Authentication Servers`.

&nbsp;  - Set \*\*NAS IP Address\*\* to `LAN (10.10.10.1)` to ensure identity consistency.

