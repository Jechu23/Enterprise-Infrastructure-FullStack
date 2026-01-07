\# 🌐 DHCP Server Configuration



The DHCP (Dynamic Host Configuration Protocol) service is configured on the Domain Controller to automatically assign IP addresses to clients within the `PG-LAN` network.



\## 🛠️ Scope Details: "LAN\_Clients"

A new scope was created with the following parameters to ensure proper connectivity and domain integration:



| Parameter | Value |

| :--- | :--- |

| \*\*IP Range\*\* | `10.10.10.100` – `10.10.10.200` |

| \*\*Subnet Mask\*\* | `255.255.255.0` |

| \*\*Router (Gateway)\*\* | `10.10.10.1` (pfSense) |

| \*\*DNS Server\*\* | `10.10.10.10` (Local DC) |

| \*\*Domain Name\*\* | `corp.thomasbytes` |

| \*\*Lease Duration\*\* | 8 Days (Default) |



---



\## 🚀 Post-Installation Steps



\### 1. Authorization in Active Directory

In a domain environment, a DHCP server must be authorized in Active Directory to start serving IPs.

\* \*\*Action:\*\* Right-click the server name in the DHCP console and select \*\*Authorize\*\*.



\### 2. Firewall Rules (pfSense)

To ensure DHCP traffic flows correctly, the following ports were verified as open on the LAN interface:

\* \*\*UDP 67\*\* (Server)

\* \*\*UDP 68\*\* (Client)



---



\## 💻 PowerShell Verification

To check the current DHCP scopes and active leases, the following commands are used:



```powershell

\# Get all DHCP scopes

Get-DhcpServerv4Scope



\# Get active IP leases

Get-DhcpServerv4Lease -ScopeId 10.10.10.0

