\# 🔌 Network Interface Architecture \& Assignment



This document describes the logical and physical mapping of the network interfaces for the \*\*PF-GW-01\*\* appliance. Proper interface alignment is critical for maintaining security boundaries between the WAN, LAN, and DMZ segments.



\## 🏗️ Logical Mapping (OS Level)



In pfSense (FreeBSD), the interfaces use the `vmx` driver, optimized for VMware paravirtualization. The assignments are as follows:



| pfSense Interface | OS Device | IP Address | MAC Address (Last 4) | Connected Port Group |

| :--- | :--- | :--- | :--- | :--- |

| \*\*WAN\*\* | `vmx0` | DHCP (192.168.4.x) | `XX:YY:ZZ:1A` | WAN-Uplink (vSwitch0) |

| \*\*LAN\*\* | `vmx1` | 172.16.10.1/24 | `XX:YY:ZZ:2B` | LAN-Core (vSwitch1) |

| \*\*OPT1\*\* | `vmx2` | 172.16.20.1/24 | `XX:YY:ZZ:3C` | OPT-Isolated (vSwitch1) |



---



\## ⚙️ Interface Optimization



To ensure high-throughput and low-latency packet processing, the following hardware offloading settings were adjusted in \*\*System > Advanced > Networking\*\*:



1\.  \*\*Hardware Checksum Offload:\*\* \*\*Disabled\*\*. (Required when using VMXNET3 drivers to prevent packet corruption).

2\.  \*\*Hardware TCP Segmentation Offload (TSO):\*\* \*\*Disabled\*\*. (Ensures pfSense handles packet fragmentation correctly for VPN traffic).

3\.  \*\*Hardware Large Receive Offload (LRO):\*\* \*\*Disabled\*\*. (Standard practice for FreeBSD-based firewalls to maintain stateful inspection integrity).







---



\## 🛡️ Interface Hardening



Each interface has been configured with security "shaper" settings:



\### 1. WAN (Untrusted)

\* \*\*Block Private Networks:\*\* Enabled (Blocks RFC1918 traffic from the internet).

\* \*\*Block Bogon Networks:\*\* Enabled (Blocks unallocated or reserved IP ranges).



\### 2. LAN (Trusted Management)

\* \*\*Static IPv4:\*\* Defined as the authoritative gateway for the `172.16.10.0/24` subnet.

\* \*\*MSS Clamping:\*\* Set to \*\*1460\*\* to prevent MTU issues for clients connecting through the VPN tunnel.



\### 3. OPT1 (Isolated Sandbox)

\* \*\*Status:\*\* Enabled, but with no services (DHCP/DNS) listening by default to maintain total isolation until explicitly configured.



---



\## 📸 Interface Status Evidence

\* \*\*Dashboard Widget:\*\* `screenshots/interface-status.png`

\* \*\*MAC Address Table:\*\* `screenshots/mac-mapping-validation.png`

