\# 🚥 Final Connectivity \& Security Matrix



This document summarizes the connectivity tests performed to validate the firewall rules and inter-VLAN routing.



\## 📊 Traffic Matrix (Source: OPT1 Client)



| Destination | Protocol | Port | Result | Logic |

| :--- | :--- | :--- | :--- | :--- |

| \*\*DC-CORP-01\*\* | TCP/UDP | 53 | \*\*PASS\*\* | DNS Resolution is required. |

| \*\*DC-CORP-01\*\* | TCP | 445 | \*\*PASS\*\* | GPO and File Access. |

| \*\*DC-CORP-01\*\* | UDP | 1812 | \*\*PASS\*\* | RADIUS Authentication. |

| \*\*LAN Other Hosts\*\*| ANY | ANY | \*\*BLOCK\*\* | Zero Trust: No lateral movement. |

| \*\*Internet\*\* | HTTPS | 443 | \*\*PASS\*\* | Regulated Web Access. |



\## 🛠️ Validation Commands

To replicate these tests, execute the following from the \*\*Win10-CLI\*\*:

```powershell

\# Test DNS Forwarding

Resolve-DnsName google.com



\# Test internal service reachability

Test-NetConnection 172.16.10.10 -Port 389

