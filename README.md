\# 🏢 End-to-End Enterprise Network Infrastructure

\### Scalable Identity, Security, and Edge Management Lab



\## 🌟 Executive Summary

This project demonstrates the deployment of a professional-grade corporate network infrastructure virtualized on \*\*VMware ESXi 6.7.0\*\*. It integrates a \*\*pfSense\*\* perimeter firewall, a \*\*Windows Server 2016\*\* identity core (Active Directory), and centralized \*\*RADIUS\*\* authentication. The goal is to simulate a real-world enterprise environment providing secure access to Windows and Linux endpoints.



---



\## 🛠️ Technology Stack

\* \*\*Hypervisor:\*\* VMware ESXi 6.7.0

\* \*\*Firewall/Routing:\*\* pfSense (WAN, LAN, OPT segments)

\* \*\*Identity:\*\* Microsoft Active Directory Domain Services (AD DS)

\* \*\*Core Services:\*\* DNS, DHCP, Group Policy (GPO)

\* \*\*Security:\*\* Network Policy Server (NPS) / RADIUS

\* \*\*Endpoints:\*\* Windows 10 Pro \& Ubuntu Desktop/Server



---



\## 📐 Network Topology \& Architecture

The infrastructure is segmented into three main zones:

1\.  \*\*WAN:\*\* External connectivity.

2\.  \*\*LAN (10.10.10.0/24):\*\* Trusted zone for Domain Controllers and Servers.

3\.  \*\*OPT (10.10.20.0/24):\*\* Isolated zone for guest/testing endpoints.







---



\## 📂 Project Modules



\### \[01. Virtualization Layer (ESXi)](./01-Virtualization-ESXi/)

\* Virtual Switch (vSwitch) configuration.

\* Port Group isolation for WAN/LAN/OPT.

\* Resource allocation for high availability.



\### \[02. Network \& Perimeter (pfSense)](./02-Network-pfSense/)

\* Interface configuration (WAN/LAN/OPT).

\* Firewall Rules (Aliases, NAT, and Blocking Inter-VLAN traffic).

\* VPN \& Gateway monitoring.



\### \[03. Identity Services (Active Directory)](./03-Active-Directory-Core/)

\* Forest/Domain promotion (`corp.thomasbytes`).

\* DNS Forwarders and Reverse Lookup Zones.

\* DHCP Scopes \& Authorization.

\* \*\*GPO Implementation:\*\* Password policies, RDP restrictions, and desktop management.



\### \[04. Centralized Authentication (RADIUS)](./04-Security-Radius/)

\* NPS installation and AD Registration.

\* RADIUS Client setup for pfSense integration.

\* Connection Request \& Network Access Policies.



\### \[05. Client Integration](./05-Endpoints-Integration/)

\* Joining Windows 10 to the domain.

\* Linux (Ubuntu) integration using SSSD/Realm.

\* Verification of GPO application and user permissions.



---



\## 🚀 Key Achievements

\* Implemented \*\*Least Privilege\*\* access via GPOs.

\* Configured \*\*DNS Sinkholing\*\* and Forwarding for secure browsing.

\* Centralized all network authentication via \*\*RADIUS\*\*, reducing local credential overhead.

\* Documented the complete \*\*ESXi networking\*\* stack for modular scalability.



---

\*\*Author:\*\* Jechu23  

\*\*Status:\*\* In Progress / Completed 🛠️

