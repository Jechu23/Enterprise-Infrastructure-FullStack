\# 🌐 Network Addressing Plan


This document describes the current network configuration of the laboratory.



\## 🏁 Network Segmentation



\### 🌎 WAN

\* \*\*Subnet:\*\* `192.168.4.0/22`

\* \*\*Gateway:\*\* `192.168.4.1` (ISP Router)

\* \*\*Assignment:\*\* DHCP



\### 💻 LAN

\* \*\*Subnet:\*\* `10.10.10.0/24`

\* \*\*Gateway:\*\* `10.10.10.1` (pfSense)

\* \*\*Primary DNS:\*\* `10.10.10.10` (Windows Server DC)



\### 🛡️ DMZ

\* \*\*Subnet:\*\* `10.10.20.0/24`

\* \*\*Gateway:\*\* `10.10.20.1` (pfSense)



---

*Documentation generated for the pfSense Enterprise Lab.*

