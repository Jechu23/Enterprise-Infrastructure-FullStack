# 🌐 Enterprise DHCP Server Configuration

The **Dynamic Host Configuration Protocol (DHCP)** service is hosted on the primary Domain Controller (`DC-CORP-01`). It is engineered to provide automated, conflict-free IP addressing for the workstation and guest segments while ensuring full integration with Active Directory DNS for dynamic updates.

## 🛠️ Scope Architecture: "OPT_Workstations"

A dedicated scope was provisioned to service the isolated client segment (OPT1), ensuring that all endpoints receive the correct routing and identity resolution parameters:

| Parameter | Value | Purpose |
| :--- | :--- | :--- |
| **Network ID** | `172.16.20.0/24` | Target segment for client workloads. |
| **Address Pool** | `172.16.20.100 – 172.16.20.200` | Reserved range for dynamic allocation. |
| **Subnet Mask** | `255.255.255.0` | Standard Class C masking. |
| **Router (Gateway)** | `172.16.20.1` | pfSense Virtual Interface for the OPT zone. |
| **DNS Server** | `172.16.10.10` | Authoritative DC for internal name resolution. |
| **Domain Name** | `corp.thomasbytes.ca` | Full FQDN for DNS suffix search lists. |
| **Lease Duration** | 8 Days | Optimized for stable workstation environments. |

---

## 🚀 Enterprise Deployment Workflow

### 1. Active Directory Authorization
In an enterprise forest, rogue DHCP servers can disrupt network operations. To prevent this, the DHCP role was explicitly **Authorized** within Active Directory.
* **Mechanism:** The server’s IP and FQDN were added to the `DhcpServers` object in the AD Configuration partition, allowing the service to bind to the network interfaces.

### 2. DNS Dynamic Update Registration
The scope is configured to automatically register client **A (Host)** and **PTR (Pointer)** records in DNS. This ensures that even if a workstation's IP changes, it remains reachable via its hostname, which is critical for remote management and GPO application.

### 3. Network Relay & Firewalling
Since the DHCP server resides in the **LAN (172.16.10.x)** but serves the **OPT1 (172.16.20.x)** segment, the following were verified:
* **DHCP Relay:** Configured on pfSense to forward broadcast requests to the unicast address of the DC.
* **Port Enforcement:** UDP Ports **67 (Server)** and **68 (Client)** are permitted in the pfSense firewall rules.



---

## 💻 Administrative PowerShell Verification

To audit the scope health and monitor active address utilization, the following PowerShell snippets are utilized:

```powershell
# Retrieve all configured IPv4 scopes and their status
Get-DhcpServerv4Scope

# List active client leases to verify distribution and hostnames
Get-DhcpServerv4Lease -ScopeId 172.16.20.0

# Verify the DHCP server's authorization status in AD
Get-DhcpServerInDC